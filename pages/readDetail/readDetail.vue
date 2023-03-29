<template>
	<view class="main">
		<my-navBack backColor="white"  background="linear-gradient(90deg, #0d0d0d, #595959)" :title="mode=='insert'?'添加文章':'修改文章'"></my-navBack>
		<view class="header">	
			
			
		</view>
		<view class="body">
			<view class="content" style="padding: 50rpx;"> 
				<uni-forms validate-trigger="bind" :rules="readRules"  ref="readDetail" :modelValue="reads" labelWidth="150">
					<!-- 	<uni-forms-item   name="cikuTypeId">
							<uni-easyinput   type="hidden" v-model="formData.selectCikuType.cikuTypeId"  />
						</uni-forms-item> -->
					<uni-forms-item  required label="文章标题" name="name">
				
						<uni-easyinput
							@blur="binddata('name', reads.name, 'readDetail')"
							:trim="true"
							type="text"
							placeholder="请输入标题"
							v-model="reads.name"
						>
							
						</uni-easyinput>
					</uni-forms-item>
					<uni-forms-item  required label="文章作者" name="author">
						<uni-easyinput
						@blur="binddata('author', reads.author, 'readDetail')"
							:trim="true"
							type="text"
							placeholder="请输入作者"
							v-model="reads.author"
						>
							
						</uni-easyinput>
					</uni-forms-item>
					<uni-forms-item  required label="文章简介" name="brief">
						<uni-easyinput
						@blur="binddata('brief', reads.brief, 'readDetail')"
							:trim="true"
							type="textarea"
							placeholder="请输入简介"
							v-model="reads.brief"
						>
							
						</uni-easyinput>
					</uni-forms-item>
					<uni-forms-item   label="文章封面" >
						<view style="width: 30%;">
							<uni-file-picker
								@select="readCoverFileSelect"
								@delete="readCoverFileDelete"
								ref="importFileByCiku"
								limit="1"
								file-mediatype="image"
								title="最多选择1个文件"
								fileExtname="jpg,png,gif"
								:modelValue="imageList"
							></uni-file-picker>
						</view>
							
						
						
					</uni-forms-item>
					<uni-forms-item  required label="文章内容" name="essay">
						
						<uni-easyinput
							@blur="binddata('essay', reads.essay, 'readDetail')"
							textAreaStyle="height:800rpx;font-size:33rpx;line-height:70rpx;-webkit-box-orient: vertical;word-break: break-all;;word-wrap: break-word;
		word-spacing:3rpx;padding: 20rpx;text-indent: 1rem;width:100%"
							type="textarea"
							
							placeholder="请输入内容"
							:maxlength="-1"
							v-model="reads.essay"
						>
							
						</uni-easyinput>
					</uni-forms-item>
				</uni-forms>
				<button type="default"  @click="updateRead" v-if="mode=='update'" style="color: white;background-color: rgb(199, 133, 0);margin-top: 40rpx;">修改文章</button>
				<button type="default" @click="insertRead"  v-else style="color: white;background-color: rgb(199, 133, 0);margin-top: 40rpx;">添加文章</button>
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
				cikuType:undefined,
				cikuTypeSub:undefined,
				current:undefined,
				mode:"insert",
				imageList:[],
				reads:{
					name:'',
					author:'',
					brief:'',
					essay:'',
					image:''
				},
				readsBackUp:{
					name:'',
					author:'',
					brief:'',
					essay:'',
					image:''
				},
				readRules: {
					name: {
						label: '文章标题',
						rules: [
							{
								required: true,
								errorMessage: '{label}不能为空，请填写'
							},
							// 对name字段进行长度验证
							{
								minLength: 1,
								maxLength: 50,
								errorMessage: '{label}长度在 {minLength} 到 {maxLength} 个字符'
							},
							{
								format: 'string',
								errorMessage: '{label}必须为字符串'
							},
							{
								validateFunction: function(rule, value, data, callback) {}
							}
						]
					},
					author: {
						label: '文章作者',
						rules: [
							{
								required: true,
								errorMessage: '{label}不能为空，请填写'
							},
							// 对name字段进行长度验证
							{
								minLength: 1,
								maxLength: 50,
								errorMessage: '{label}长度在 {minLength} 到 {maxLength} 个字符'
							},
							{
								format: 'string',
								errorMessage: '{label}必须为字符串'
							},
							{
								validateFunction: function(rule, value, data, callback) {}
							}
						]
					},
					brief: {
						label: '文章简介',
						rules: [
							{
								required: true,
								errorMessage: '{label}不能为空，请填写'
							},
							// 对name字段进行长度验证
							{
								minLength: 1,
								maxLength: 250,
								errorMessage: '{label}长度在 {minLength} 到 {maxLength} 个字符'
							},
							{
								format: 'string',
								errorMessage: '{label}必须为字符串'
							},
							{
								validateFunction: function(rule, value, data, callback) {}
							}
						]
					},
					essay: {
						label: '文章内容',
						rules: [
							{
								required: true,
								errorMessage: '{label}不能为空，请填写'
							},
							// 对name字段进行长度验证
							{
								minLength: 1,
								
								errorMessage: '{label}长度大于 {minLength}个字符'
							},
							{
								format: 'string',
								errorMessage: '{label}必须为字符串'
							},
							{
								validateFunction: function(rule, value, data, callback) {}
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
			if(param.readType!=undefined){
				this.readType=parseInt(param.readType); 
			}
			if(param.readTypeSub!=undefined){
				this.readTypeSub=parseInt(param.readTypeSub) ;
			}
			if(param.current!=undefined){
				this.current = parseInt(param.current);
			}
			
			
		},
		onShow() {
			this.globalData=getApp().globalData;
			if(this.readType==undefined||this.readTypeSub==undefined){
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
			uni.setStorageSync("readType",this.readType);
			uni.setStorageSync("readTypeSub",this.readTypeSub);
			uni.setStorageSync("readCurrent",this.current);
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
			this.$refs['readDetail'].setRules(this.readRules);
		},
		methods: {
			//添加文章 start
			insertRead(){
				this.$refs['readDetail']
					.validate()
					.then(res => {
						console.log('表单数据信息：', res);
						
						// 使用 Promise then/catch 方式调用
						this.reads.essay = this.globalData.Base64.encode(this.reads.essay);
						uni.request({
							url:this.globalData.url+"adminRead/insert",
							method:'POST',
							header:this.globalData.header.normal,
							dataType:"json",
							data:{
								read:JSON.stringify(this.reads),
								readType:this.readType,
								readTypeSub:this.readTypeSub
							}
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								uni.showToast({
									title:"添加成功"
								})
								this.reads=JSON.parse(JSON.stringify(this.readsBackUp));
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
			//添加单词 end
			
			//修改单词 start
			updateRead(){
				this.reads.essay = this.globalData.Base64.encode(this.reads.essay);
				uni.request({
					url:this.globalData.url+"adminRead/update",
					method:'POST',
					header:this.globalData.header.normal,
					dataType:"json",
					data:{
						read:JSON.stringify(this.reads),
						readType:this.readType,
						readTypeSub:this.readTypeSub
					}
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
			//修改单词 end
			//上传图片 start
			readCoverFileSelect(e){
				this.uploadfile(e.tempFilePaths[0]).then((res)=>{
					this.reads.image = res;
				})
			},
			//上传图片 end
			//取消上传图片 start
			readCoverFileDelete(e){
				this.reads.image="";
				this.imageList=[];
			},
			//取消上传图片 end
			//上传文件
			uploadfile(tempFile){
				// 使用 Promise then/catch 方式调用
				return new Promise((resolve,reject)=>{
					uni.uploadFile({
						url: this.globalData.url + 'adminFile/uploadReadImage',
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
				if(this.mode=="update"){
					this.loadNetData();
				}
				
			},
			//加载数据 end
			//加载服务器数据 start
			loadNetData(){
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminRead/getOneById",
							method:'POST',
							header:this.globalData.header.normal,
							dataType:"json",
							data:{
								readType:this.readType,
								readTypeSub:this.readTypeSub,
								id:this.id
							}
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.reads = data.data;
								this.reads.essay = this.globalData.Base64.decode(this.reads.essay,1);
								this.readsBackUp = JSON.parse(JSON.stringify(this.reads));
								this.imageList=[{url:`${this.globalData.url}static/image/readImage/${this.reads.image}`}];
								

								console.log(this.reads);
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
