<template>
	<view class="main">
		<my-navBack backColor="white"  background="linear-gradient(90deg, #0d0d0d, #595959)" :title="mode=='insert'?'添加积分来源类别':'修改积分来源类别'"></my-navBack>
		<view class="header">	
			
			
		</view>
		<view class="body">
			<view class="content" style="padding: 50rpx;"> 
				<uni-forms validate-trigger="bind" :rules="scoreSourceRules"  ref="scoreSourceDetail" :modelValue="scoreSources" labelWidth="150">
					<!-- 	<uni-forms-item   name="cikuTypeId">
							<uni-easyinput   type="hidden" v-model="formData.selectCikuType.cikuTypeId"  />
						</uni-forms-item> -->
					<uni-forms-item  required label="类别名称" name="dsc">
				
						<uni-easyinput
							@blur="binddata('dsc', scoreSources.dsc, 'scoreSourceDetail')"
							:trim="true"
							type="text"
							placeholder="请输入标题"
							v-model="scoreSources.dsc"
						>
							
						</uni-easyinput>
					</uni-forms-item>
					<uni-forms-item  required label="默认值" name="defaults">
						<uni-easyinput
						@blur="binddata('defaults', scoreSources.defaults, 'scoreSourceDetail')"
							:trim="true"
							type="number"
							placeholder="请输入默认值"
							v-model="scoreSources.defaults"
						>
							
						</uni-easyinput>
					</uni-forms-item>
					
						
					<uni-forms-item  required label="类别类型" name="type">
						<uni-data-checkbox
						@blur="binddata('type', scoreSources.type,'scoreSourcesDetail')"
							:check="1"
							titleColor="black"
							radioSize="35rpx"
							radioFontSize="35rpx"
							titleFontSize="35rpx"
							v-model="scoreSources.type"
							:localdata="type"
						></uni-data-checkbox>
					</uni-forms-item>
				</uni-forms>
				<button type="default"  @click="updateScoreSource" v-if="mode=='update'" style="color: white;background-color: rgb(199, 133, 0);margin-top: 40rpx;">修改积分来源类别</button>
				<button type="default" @click="insertScoreSource"  v-else style="color: white;background-color: rgb(199, 133, 0);margin-top: 40rpx;">添加积分来源类别</button>
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
				index:undefined,
				current:undefined,
				mode:"insert",
				type: [{
									text: '加+',
									value: 0
				
								}, {
									text: '减-',
									value: 1,
				
								}
							
								],
				scoreSources:{
					dsc:'',
					defaults:null,
					type:0,
				},
				scoreSourcesBackUp:{
					dsc:'',
					defaults:null,
					type:0,
				},
				scoreSourceRules: {
					dsc: {
						label: '类型名称',
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
								validateFunction: (rule, value, data, callback) => {
									// 异步需要返回 Promise 对象
									return new Promise((resolve, reject) => {
									
											// console.log(data);
											uni.request({
												url: this.globalData.url + 'adminScore/getScoreSourceTypeExist',
												method: 'POST',
												dataType: 'json',
												header: {
													'content-type': this.globalData.contentType.normalData,
													token: uni.getStorageSync('token')
												},
												data: {
													
													dsc: value
												},
												success: res => {
													let data = res.data;
													if (data.code == 200) {
														reject(new Error(data.message));
													} else if (data.code == 520) {
														this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
													} else {
														resolve();
													}
												},
												fail: error => {
													reject(new Error('请求数据错误'));
												}
											});
										
									});
								}
							}
						]
					},
					defaults: {
						label: '默认值',
						rules: [
							{
								required: true,
								errorMessage: '{label}不能为空，请填写'
							},

							{
								format: 'number',
								errorMessage: '{label}必须为数字'
							},
							{
								validateFunction: (rule, value, data, callback)=> {
									if(value<0){
										this.scoreSources.type=1;
									}else if(value>0){
										this.scoreSources.type=0;
									}
									this.scoreSources.defaults=Math.abs(value);
								}
							}
						]
					},
					type: {
						label: '类别类型',
						rules: [
							{
								required: true,
								errorMessage: '{label}不能为空，请填写'
							},
							// 对name字段进行长度验证
							
							{
								format: 'number',
								errorMessage: '{label}必须为数字'
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
			if(param.index!=undefined){
				this.index = parseInt(param.index);
			}
			if(param.current!=undefined){
				this.current = parseInt(param.current);
			}
			
			
		},
		onShow() {
			this.globalData=getApp().globalData;
			if(this.index==undefined){
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
			uni.setStorageSync("scoreIndex",this.index);
			uni.setStorageSync("scoreSourceCurrent",this.current);
			if(this.id!=undefined||this.id==0){
				this.mode="update";
			}else{
				this.mode="insert";
			}
			
			if(this.globalData.checkLogin()){
				this.loadData();
			}
		},
		onScoreSourcey() {
			this.$refs['scoreSourceDetail'].setRules(this.scoreSourceRules);
		},
		methods: {
			//添加文章 start
			insertScoreSource(){
				this.$refs['scoreSourceDetail']
					.validate()
					.then(res => {
						console.log('表单数据信息：', res);
						
						// 使用 Promise then/catch 方式调用
				
						uni.request({
							url:this.globalData.url+"adminScore/insertScoreSource",
							method:'POST',
							header:this.globalData.header.json,
							dataType:"json",
							data:this.scoreSources,
							
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								uni.showToast({
									title:"添加成功"
								})
								this.scoreSources=JSON.parse(JSON.stringify(this.scoreSourcesBackUp));
		
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
			updateScoreSource(){
				
				uni.request({
					url:this.globalData.url+"adminScore/updateScoreSource",
					method:'POST',
					header:this.globalData.header.json,
					dataType:"json",
					data:this.scoreSources,
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
							url:this.globalData.url+"adminScore/getOneScoreSourceTypeById",
							method:'POST',
							header:this.globalData.header.normal,
							dataType:"json",
							data:{
								id:this.id
							}
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.scoreSources = data.data;
							
								this.scoreSourcesBackUp = JSON.parse(JSON.stringify(this.scoreSources));
							
								

								console.log(this.scoreSources);
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
