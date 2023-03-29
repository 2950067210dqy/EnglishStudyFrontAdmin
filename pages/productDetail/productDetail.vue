<template>
	<view class="main">
		<my-navBack backColor="white"  background="linear-gradient(90deg, #0d0d0d, #595959)" :title="mode=='insert'?'添加商品':'修改商品'"></my-navBack>
		<view class="header">	
			
			
		</view>
		<view class="body">
			<view class="content" style="padding: 50rpx;"> 
				<uni-forms validate-trigger="bind" :rules="productRules"  ref="productDetail" :modelValue="products" labelWidth="150">
					<!-- 	<uni-forms-item   name="cikuTypeId">
							<uni-easyinput   type="hidden" v-model="formData.selectCikuType.cikuTypeId"  />
						</uni-forms-item> -->
					<view style="vertical-align: middle;padding: 50rpx;">
						<view  style="display: inline-block;vertical-align: middle;">
							商家：{{products.user.username}}
						</view>
						<view style="display: inline-block;vertical-align: middle;margin-left: 20rpx;">
							<image @error="basicProductsUserImageError" style="width:100rpx;height: 100rpx;border-radius: 50%;" :src="products.user.headimage"></image>
						</view>
						
					</view>	
					<uni-forms-item  required label="商品名" name="title">
					
						<uni-easyinput
							@blur="binddata('title', products.product.title, 'productDetail')"
							:trim="true"
							textAreaStyle="height:800rpx;font-size:33rpx;line-height:70rpx;-webkit-box-orient: vertical;word-break: break-all;;word-wrap: break-word;
							word-spacing:3rpx;padding: 20rpx;text-indent: 1rem;width:100%"
							type="textarea"
							placeholder="请输入商品名"
							v-model="products.product.title"
						>
							
						</uni-easyinput>
					</uni-forms-item>
					<uni-forms-item  required label="商品价格(单位积分:1元=10积分)" name="price">
						<uni-easyinput
						@blur="binddata('price', products.product.price, 'productDetail')"
							:trim="true"
							type="number"
							placeholder="请输入商品价格"
							v-model="products.product.price"
						>
							
						</uni-easyinput>
					</uni-forms-item>
					<uni-forms-item  required label="商品库存" name="num">
						<uni-easyinput
						@blur="binddata('num', products.product.num, 'productDetail')"
							:trim="true"
							type="number"
							placeholder="请输入商品库存"
							v-model="products.product.num"
						>
							
						</uni-easyinput>
					</uni-forms-item>
					<uni-forms-item required  label="商品封面图片"  >
						<view style="width: 30%;" >
							<uni-file-picker
							
								@select="productCoverFileSelect"
								@delete="productCoverFileDelete"
								ref="importFileByCiku"
								limit="1"
								file-mediatype="image"
								title="最多选择1个文件"
								fileExtname="jpg,png,gif"
								v-model="imageList"
							></uni-file-picker>
						</view>
							
						
						
					</uni-forms-item>
					
				</uni-forms>
				<button type="default"  @click="updateProduct" v-if="mode=='update'" style="color: white;background-color: rgb(199, 133, 0);margin-top: 40rpx;">修改商品</button>
				<button type="default" @click="insertProduct"  v-else style="color: white;background-color: rgb(199, 133, 0);margin-top: 40rpx;">添加商品</button>
			</view>
		</view>
		
		<view class="footer">
			
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				globalData:{},
				id:undefined,
				pType:undefined,
				pTypeSub:undefined,
				current:undefined,
				mode:"insert",
				imageList:[],
				products:{
					user:{
						
					},
					product:{
						title:'',
						price:0,
						num:0,
						imgAddre:'',
						productAddre:'',
						ptypeid:0,
						ptypesubid:0,
						merchantId:0,
					}
				
				},
				productsBackUp:{
					user:{
						
					},
					product:{
						title:'',
						price:0,
						num:0,
						imgAddre:'',
						productAddre:'',
						ptypeid:0,
						ptypesubid:0,
						merchantId:0,
					}
				},
				productRules: {
					title: {
						label: '商品名称',
						rules: [
							
							// 对name字段进行长度验证
							{
								minLength: 1,
								maxLength: 550,
								errorMessage: '{label}长度在 {minLength} 到 {maxLength} 个字符'
							},
							{
								format: 'string',
								errorMessage: '{label}必须为字符串'
							},
							{
								validateFunction: (rule, value, data, callback)=> {
									if(value.trim().length==0||value.trim()==''){
										callback('{label}不能为空')
									}else{
										callback();
									}
								}
							}
						]
					},
					price: {
						label: '商品价格',
						rules: [
							
							// 对name字段进行长度验证
						
							
							{
								validateFunction: (rule, value, data, callback)=> {
									let min=0;
									let max = 10000000;
									console.log(value);
									if(value==null||value==''){
										callback('{label}不能为空')
									}else if(value<min||value>max){
										callback('{label}取值必须在'+min+'-'+max+'之间')
									}else{
										callback();
									}
								}
							}
						]
					},
					num: {
						label: '商品库存',
						rules: [
							
							// 对name字段进行长度验证
							
							
							{
								validateFunction: (rule, value, data, callback)=> {
									let min=0;
									let max = 100000;
									console.log(value);
									if(value==null||value==''){
										callback('{label}不能为空')
									}else if(value<min||value>max){
										callback('{label}取值必须在'+min+'-'+max+'之间')
									}else{
										callback();
									}
								}
							}
						]
					},
				
				},
				//
			}
		},
		onLoad(param) {
			this.globalData=getApp().globalData;
			if(param.id!=undefined){
				this.id = parseInt(param.id);
			}
			if(param.pType!=undefined){
				this.pType=parseInt(param.pType); 
			}
			if(param.pTypeSub!=undefined){
				this.pTypeSub=parseInt(param.pTypeSub) ;
			}
			if(param.current!=undefined){
				this.current = parseInt(param.current);
			}
			
			
		},
		onShow() {
			this.globalData=getApp().globalData;
			if(this.pType==undefined||this.pTypeSub==undefined){
				uni.showModal({
					title:"错误",
					content:"参数错误",
					success: (res) => {
						uni.redirectTo({
							url:"/pages/index/index"
						})
					}
				})
			}
			uni.setStorageSync("pType",this.pType);
			uni.setStorageSync("pTypeSub",this.pTypeSub);
			uni.setStorageSync("productCurrent",this.current);
			if(this.id!=undefined||this.id==0){
				this.mode="update";
			}else{
				this.mode="insert";
			}
			
			if(this.globalData.checkLogin()){
				this.loadData();
			}
		},
		onReady() {
			this.$refs['productDetail'].setRules(this.productRules);
		},
		methods: {
			//图片错误 start
			basicProductsUserImageError(){
				this.products.user.headimage = this.globalData.defaultHeadImage;
			},
			//图片错误 end
			//添加商品 start
			insertProduct(){
				this.$refs['productDetail']
					.validate()
					.then(res => {
						console.log('表单数据信息：', res);
						
						// 使用 Promise then/catch 方式调用
						if(this.products.product.imgAddre.trim()==''||this.products.product.imgAddre.trim().length==0){
							uni.showToast({
								title:'商品图片不能为空！',
							});
							return;
						}
						uni.request({
							url:this.globalData.url+"adminProduct/insert",
							method:'POST',
							header:this.globalData.header.json,
							dataType:"json",
							data:this.products.product
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								uni.showToast({
									title:"添加成功"
								})
								this.products=JSON.parse(JSON.stringify(this.productsBackUp));
								this.imageList=[];
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								
							} else {	
								uni.showToast({
									title:data.message,
									icon:'error'
								})
								
							}
						}).catch((e)=>{
							uni.showToast({
								title:"请求错误",
								icon:'error'
							})
							console.log(e);
							
						})		
					})
					.catch(err => {
						uni.showToast({
							icon: 'error',
							title: '上传数据格式错误'
						});
						console.log('表单错误信息：', err);
					});
			},
			//添加商品 end
			
			//修改商品 start
			updateProduct(){
				
				uni.request({
					url:this.globalData.url+"adminProduct/update",
					method:'POST',
					header:this.globalData.header.json,
					dataType:"json",
					data:this.products.product,
					
				}).then((res)=>{
					let data =res.data;
					if (data.code == 200) {
						// console.log(this.globalData.Base64.decode( data.data.sentences));
						uni.showToast({
							title:"修改成功"
						})
						setTimeout(()=>{
							uni.navigateBack();
						},500)
					} else if (data.code == 520) {
						this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
						
					} else {	
						uni.showToast({
							title:data.message,
							icon:'error'
						})
						
					}
				}).catch((e)=>{
					uni.showToast({
						title:"请求错误",
						icon:'error'
					})
					console.log(e);
					
				})		
			},
			//修改商品 end
			//上传图片 start
			productCoverFileSelect(e){
				this.uploadfile(e.tempFilePaths[0]).then((res)=>{
					this.products.product.imgAddre = res;
				})
			},
			//上传图片 end
			//取消上传图片 start
			productCoverFileDelete(e){
				this.products.product.imgAddre="";
				this.imageList=[];
			},
			//取消上传图片 end
			//上传文件
			uploadfile(tempFile){
				// 使用 Promise then/catch 方式调用
				return new Promise((resolve,reject)=>{
					uni.uploadFile({
						url: this.globalData.url + 'adminFile/uploadProductImage',
						filePath: tempFile,
						name: 'file',
						header: {
							token: uni.getStorageSync('token')
						},
					})
					.then(res => {
						// 此处的 res 参数，与使用默认方式调用时 success 回调中的 res 参数一致
						// console.log(res.data);
						
						let data = JSON.parse(res.data);
						if (data.code == 200) {
							uni.showToast({
								title: '上传成功'
							});
							resolve(data.data);
						} else if (data.code == 520) {
							this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
							reject(data.message);
						} else {
							uni.showToast({
								title: data.message,
								icon: 'error'
							});
							reject(data.message);
						}
								
						// this.resetFormData();
					})
					.catch(err => {
						// 此处的 err 参数，与使用默认方式调用时 fail 回调中的 err 参数一致
						
						// console.log(err);
						uni.showToast({
							title: '提交错误',
							icon: 'error'
						});
						reject(err);
						// this.resetFormData();
					});
				});	
			},
			//上传文件 end
			//加载数据 start
			loadData(){
				this.products.product.ptypeid=this.pType;
				this.products.product.ptypesubid=this.pTypeSub;
				this.productsBackUp.product.ptypeid=this.pType;
				this.productsBackUp.product.ptypesubid=this.pTypeSub;
				if(this.mode=="update"){
					this.loadNetData();
				}else{
					this.products.user = JSON.parse(uni.getStorageSync("user"));
					this.products.user.headimage = this.globalData.url+"static/image/headImage/"+this.products.user.headimage;
					this.products.product.merchantId=this.products.user.id;
					this.productsBackUp.user = JSON.parse(uni.getStorageSync("user"));
					this.productsBackUp.user.headimage = this.globalData.url+"static/image/headImage/"+this.productsBackUp.user.headimage;
					this.productsBackUp.product.merchantId=this.products.user.id;
				}
				
			},
			//加载数据 end
			//加载服务器数据 start
			loadNetData(){
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminProduct/getOneById",
							method:'POST',
							header:this.globalData.header.normal,
							dataType:"json",
							data:{
								pType:this.pType,
								pTypeSub:this.pTypeSub,
								id:this.id
							}
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.products = data.data;
								this.products.product.imgAddre =this.products.product.imgAddre.indexOf('//')==-1?(this.globalData.url+'static/image/productImage/'+this.products.product.imgAddre):this.products.product.imgAddre;
									
								this.products.user.headimage = this.globalData.url+"static/image/headImage/"+this.products.user.headimage;
								
								this.productsBackUp = JSON.parse(JSON.stringify(this.products));
								this.imageList=[{url:this.products.product.imgAddre}];
								

								console.log(this.products);
								resolve();
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {
								console.log(data.message);
								resolve(data.message);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})		
				})
			},
			//加载服务器数据 end
		}
	}
</script>

<style>
 .header{

}
.body{

}
.content{

}
.footer{

}

</style>
