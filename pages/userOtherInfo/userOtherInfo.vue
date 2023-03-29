<template>
	<view class="main" :style="mainStyle">
		<my-navBack backColor="white"  background="linear-gradient(90deg, #0d0d0d, #595959)" :title="user.id==0?user.type==0?'添加用户':'添加管理员':user.type==0?'修改用户':'修改管理员'"></my-navBack>
		<view class="header">	
			<image style="width: 350rpx;height: 350rpx;margin-top: 50rpx;" :src="headimageUrl" @error="imageError"
							@tap="atvarClick"></image>
			
		</view>
		<view class="body">
			<view class="content" style="width: 50%;padding: 50rpx;"> 
				<uni-forms ref="user" :modelValue="user">
					<uni-forms-item labelWidth="70" labelColor="white" labelFontSize="35rpx" label="请选择头像:" name="headimageList">
						<uni-file-picker
							@blur="binddata('headimageList', user.headimageValue, 'user')"
							@select="headimageSelect"
							@success="headimageSucess"
							@fail="headimageFail"
							@delete="headimageDelete"
							@progress="headimageProgress"
							ref="importheadimage"
							limit="1"
							fileMediatype="image"
							title="最多选择1张图片(png,jpg,gif)"
							titleColor="white"
							titleFontSize="35rpx"
							fileExtname="png,jpg,gif"
							v-model="user.headimageList"
						></uni-file-picker>
					</uni-forms-item>
					<uni-forms-item name="username">
						<uni-easyinput
						@blur="binddata('username', user.username,'user')"
							placeholderStyle="color:rgb(234, 234, 234);font-size:35rpx"
							color="white"
							fontSize="35rpx"
							prefixIcon="contact"
							height="90rpx"
							type="text"
							v-model="user.username"
							
							placeholder="请输入用户名"
						/>
					</uni-forms-item>
					<uni-forms-item name="password">
						<uni-easyinput
						@blur="binddata('password', user.password,'user')"
							placeholderStyle="color:rgb(234, 234, 234);font-size:35rpx"
							color="white"
							fontSize="35rpx"
							prefixIcon="eye"
							height="90rpx"
							type="password"
							
							v-model="user.password"
							placeholder="请输入密码"
						/>
					</uni-forms-item>
					
					<uni-forms-item name="phone">
						<uni-easyinput
						@blur="binddata('phone', user.phone,'user')"
							placeholderStyle="color:rgb(234, 234, 234);font-size:35rpx"
							color="white"
							fontSize="35rpx"
							prefixIcon="phone"
							height="90rpx"
							
							type="text"
							v-model="user.phone"
							placeholder="请输入电话号码"
						>
						
						</uni-easyinput>
					</uni-forms-item>
					
					<uni-forms-item name="email">
						<uni-easyinput
						@blur="binddata('email', user.email,'user')"
							placeholderStyle="color:rgb(234, 234, 234);font-size:35rpx"
							color="white"
							fontSize="35rpx"
							prefixIcon="email"
							height="90rpx"
							type="text"
							v-model="user.email"
							placeholder="请输入邮箱"
							
						>
						
					
						</uni-easyinput>
					</uni-forms-item>
				
					<uni-forms-item name="name">
						<uni-easyinput
						@blur="binddata('name', user.name,'user')"
							placeholderStyle="color:rgb(234, 234, 234);font-size:35rpx"
							color="white"
							fontSize="35rpx"
							prefixIcon="chatbubble-filled"
							height="90rpx"
							type="text"
							v-model="user.name"
							placeholder="请输入昵称"
						/>
					</uni-forms-item>
					<uni-forms-item name="sex">
						<uni-data-checkbox
						@blur="binddata('sex', user.sex,'user')"
							:check="1"
							title="性别:"
							titleColor="rgb(234, 234, 234)"
							radioSize="35rpx"
							radioFontSize="35rpx"
							titleFontSize="35rpx"
							v-model="user.sex"
							:localdata="sex"
						></uni-data-checkbox>
					</uni-forms-item>
					<uni-forms-item label="出生年月：" labelWidth="70" labelColor="white" labelFontSize="35rpx" name="birthday">
						<uni-datetime-picker
							@blur="binddata('birthday', user.birthday,'user')"
							start="1900-01-01"
							:end="globalData.getDateTime(19)"
							@change="dateChange"
							type="date"
							:clear-icon="false"
							v-model="user.birthday"
						/>
					</uni-forms-item>
					<uni-forms-item labelWidth="70" labelColor="white" labelFontSize="35rpx" label="年龄:" name="age">
						<uni-easyinput
							@blur="binddata('age', user.age,'user')"
							disabled
							placeholderStyle="color:rgb(234, 234, 234);font-size:35rpx"
							color="white"
							fontSize="35rpx"
							prefixIcon="paperplane-filled"
							height="90rpx"
							type="text"
							v-model="user.age"
							placeholder="请输入年龄"
						/>
					</uni-forms-item>
					
				</uni-forms>
			</view>
		</view>
		
		<view class="footer">
			
			<view v-if="user.id!=0" class="login"><button @tap="update" class="button1" type="warn">{{user.type==0?'修改用户':'修改管理员'}}</button></view>
			<view v-else class="login"><button @tap="insert" class="button1" type="primary">{{user.type==0?'添加用户':'添加管理员'}}</button></view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				globalData:{
					getDateTime:function(){}
				},
				
				user:{
					id:0,
					username:'',
					password:'',
					name:'',
					headimage:'',
					sex:'',
					birthday:'',
					age:0,
					phone:'',
					email:'',
					type:0,
					deleted:0,
					createtime:'',
					updatetime:''
				},
				rules:{
					username:{
						label:'用户名',
						rules:[
							{
								require:true,
								errorMessage: '{label}不能为空，请填写',
							},
							{
								format: 'string',
								errorMessage: '{label}只为字符'
							},
							{
								validateFunction:(rule, value, data, callback) => {
														// 异步需要返回 Promise 对象
														return new Promise((resolve, reject) => {
															let minLength = 4;
															let maxLength = 50;
															if (value.trim().length == 0) {
																reject(new Error('{label}不能为空，请填写'));
															} else if (value.trim().length > maxLength || value.trim().length < minLength) {
																reject(new Error('{label}长度应该在' + minLength +"-" + maxLength +'位'));
															} else {
																uni.request({
																		url:this.globalData.url+"user/selectByUsername",
																		method:'POST',
																		dataType:'json',
																		header:{
																			'content-type':this.globalData.contentType.normalData,
																		},
																		data:{
																			username:value
																		},
																		success: (res) => {
																			let data =res.data;
																			if(data.code==200){
																				reject(new Error('{label}'+data.message))
																			}else{
																				resolve();
																			}
																		},
																		fail: (error) => {
																			reject(new Error('请求数据错误'))
																		}
																});
															}
													
													
														});
													
								}
								
								
								
							
							}
						]
					},
					password:{
						label:'密码',
						rules:[
							{
								require:true,
								errorMessage: '{label}不能为空，请填写',
							},
							{
								format: 'string',
								errorMessage: '{label}只为字符'
							},
							{
								validateFunction: function(rule, value, data, callback) {
									let minLength = 6;
									let maxLength = 20;
					
									if (value.trim().length == 0) {
										callback('{label}不能为空，请填写');
									} else if (value.trim().length > maxLength || value.trim().length < minLength) {
										callback('{label}长度应该在' +  minLength +"-" + maxLength + '位');
									}
									 else {
										callback();
									}
								}
							}
						]
					},
					phone:{
						label:'电话号码',
						rules:[
							{
								require:true,
								errorMessage: '{label}不能为空，请填写',
							},
							{
								format: 'number',
								errorMessage: '{label}只为数字'
							},
							{
								validateFunction:(rule, value, data, callback) => {
														// 异步需要返回 Promise 对象
														return new Promise((resolve, reject) => {
															let minLength = 11;
															let maxLength = 11;
															if (value.toString().length == 0) {
																reject(new Error('{label}不能为空，请填写'))
															} else if (value.toString().length > maxLength || value.toString().length < minLength) {
																reject(new Error('{label}长度应该在' +  minLength + '位'));
															} else {
																uni.request({
																		url:this.globalData.url+"user/selectByPhone",
																		method:'POST',
																		dataType:'json',
																		header:{
																			'content-type':this.globalData.contentType.normalData,
																		},
																		data:{
																			phone:value
																		},
																		success: (res) => {
																			let data =res.data;
																			if(data.code==200){
																				reject(new Error('{label}'+data.message))
																			}else{
																				resolve();
																			}
																		},
																		fail: (error) => {
																			reject(new Error('请求数据错误'))
																		}
																});
															}
					
					
														});
					
													}
								}
					
					
						]
						
					},
					email:{
						label:'邮箱',
						rules:[
							{
								require:true,
								errorMessage: '{label}不能为空，请填写',
							},
							{
								format: 'email',
								errorMessage: '{label}只为邮箱格式'
							},
							{
								
								validateFunction:(rule, value, data, callback) => {
														// 异步需要返回 Promise 对象
														return new Promise((resolve, reject) => {
															let minLength = 4;
															let maxLength = 50;
															if (value.toString().length == 0) {
																reject(new Error('{label}不能为空，请填写'))
															} else if (value.toString().length > maxLength || value.toString().length < minLength) {
																reject(new Error('{label}长度应该在'+  minLength +"-" + maxLength + '位'));
															} else {
																uni.request({
																		url:this.globalData.url+"user/selectByEmail",
																		method:'POST',
																		dataType:'json',
																		header:{
																			'content-type':this.globalData.contentType.normalData,
																		},
																		data:{
																			email:value
																		},
																		success: (res) => {
																			let data =res.data;
																			if(data.code==200){
																				reject(new Error('{label}'+data.message))
																			}else{
																				resolve();
																			}
																		},
																		fail: (error) => {
																			reject(new Error('请求数据错误'))
																		}
																});
															}
								
								
														});
								
													}
												
					
							}
						]
					},
					name:{
						label:'昵称',
						rules:[
							{
								require:true,
								errorMessage: '{label}不能为空，请填写',
							},
							{
								format: 'string',
								errorMessage: '{label}只为字符'
							},
							{
								validateFunction: function(rule, value, data, callback) {
									let minLength = 1;
									let maxLength = 30;
									if (value.trim().length == 0) {
										callback('{label}不能为空，请填写');
									} else if (value.trim().length > maxLength || value.trim().length < minLength) {
										callback('{label}长度应该在' +  minLength +"-" + maxLength + '位');
									} else {
										callback();
									}
								}
							}
						]
					},
					sex:{
						label:'性别',
						rules:[
							{
								require:true,
								errorMessage: '{label}不能为空，请填写',
							},
							{
								format: 'number',
								errorMessage: '{label}只为数字'
							},
							{
								validateFunction: function(rule, value, data, callback) {
									let minLength = 0;
									let maxLength = 2;
									if (value == null||value == undefined) {
										callback('{label}不能为空，请填写');
									} else if (value > maxLength || value < minLength) {
										callback('{label} 异常');
									} else {
										callback();
									}
								}
							}
						]
					},
					birthday:{
						label:'出生年月',
						rules:[
							{
								require:true,
								errorMessage: '{label}不能为空，请填写',
							},
							{
								format: 'string',
								errorMessage: '{label}只为字符'
							},
							{
								validateFunction: function(rule, value, data, callback) {
									let minLength = 9;
									let maxLength = 15;
									if (value.trim().length == 0) {
										callback('{label}不能为空，请填写');
									} else if (value.trim().length > maxLength || value.trim().length < minLength) {
										callback('{label}长度应该在' +  minLength +"-" + maxLength + '位');
									} else {
										callback();
									}
								}
							}
						]
					},
					age:{
						label:'年龄',
						rules:[
							{
								require:true,
								errorMessage: '{label}不能为空，请填写',
							},
							{
								format: 'number',
								errorMessage: '{label}只为数字'
							},
							{
								validateFunction: function(rule, value, data, callback) {
									let minLength = 0;
									let maxLength = 200;
									if (value == null||value == undefined) {
										callback('{label}不能为空，请填写');
									}
									else if (value > maxLength || value < minLength) {
										callback('{label}应该在' +  minLength +"-" + maxLength + '之间');
									} else {
										callback();
									}
								}
							}
						]
					},
				},
				
				sex: [{
									text: '男',
									value: 0
				
								}, {
									text: '女',
									value: 1,
				
								}, {
									text: '保密',
									value: 2
								}],
				headimageUrl:'',
				mainStyle: {
					backgroundImage: 'url("../../static/bg2.jpg")',
					// backgroundColor: 'black',
					backgroundRepeat: 'no-repeat',
					height:'100%',
					backgroundSize: 'cover',
					position: 'relative'
				},
			}
		},
		onLoad(param) {
			this.globalData=getApp().globalData;
			if(param.uid!=undefined&&param.uid!=null&&param.uid!=0){
				this.user.id=parseInt(param.uid);
			}
			if(param.type!=undefined&&param.type!=null){
				this.user.type=parseInt(param.type);
			}
		},
		onShow() {
			this.globalData=getApp().globalData;
			if(this.globalData.checkLogin()){
				this.loadData();
			}
		},
		onReady() {
			this.$refs['user'].setRules(this.rules);
		},
		methods: {
			//insert start
			insert(){
				this.$refs['user']
					.validate()
					.then(res => {
						console.log(this.user);
						uni.request({
							url:this.globalData.url+"user/save",
							dataType:'json',
							method:'POST',
							header:{
								'content-type':this.globalData.contentType.jsonData,
								'token':uni.getStorageSync('token')
							},
							data:this.user,
							success: (res) => {
								let data = res.data;
								if(data.code==200){
									
									uni.showToast({
										title:"添加成功"
									})
						
									this.headimageUrl=this.globalData.url+'static/image/headImage/'+this.user.headimage;
									setTimeout(()=>{
										uni.navigateBack()
									},500)
									
									
									
								}else{
									uni.showToast({
										title:"添加失败"
									});
									console.log(data.message);
								}
							},
							fail: (e) => {
								console.log(e);
								
							}
						})
						
					}).catch(err => {
						uni.showToast({
							icon: 'error',
							title: '数据格式错误'
						});
						console.log('表单错误信息：', err);
					});
			},
			//insert end
			
			//update start
			update(){
		
						console.log(this.user);
						uni.request({
							url:this.globalData.url+"user/update",
							dataType:'json',
							method:'POST',
							header:{
								'content-type':this.globalData.contentType.jsonData,
								'token':uni.getStorageSync('token')
							},
							data:this.user,
							success: (res) => {
								let data = res.data;
								if(data.code==200){
									
									uni.showToast({
										title:"修改成功"
									})
									this.user =data.data;
									this.headimageUrl=this.globalData.url+'static/image/headImage/'+this.user.headimage;
									
								}else{
									uni.showToast({
										title:"修改失败"
									})
									console.log(data.message);
								}
							},
							fail: (e) => {
								console.log(e);
								
							}
						})

					
			
			},
			//update end
			//词库文件选择文件触发的方法 start
			headimageSelect(e) {
				console.log('选择文件', e);
				uni.uploadFile(
				{
					url:this.globalData.url+'file/uploadImage',
					filePath:e.tempFilePaths[0],
					name:'file',
					dataType:'json',
					header:{
					
						'token':uni.getStorageSync('token')
					},
					xhrFields: { withCredentials: true },
					success: (res) => {
						let data =JSON.parse(res.data);
						if(data.code==200){
							this.user.headimage = data.data;
						}else{
							console.log(data.message);
						}
					},
					fail: (e) => {
						console.log(e);
					}
				}
				)
			},
			//词库文件选择文件触发的方法 end
			//词库文件上传文件成功触发的方法 start
			headimageSucess(e) {
				console.log('上传文件成功', e);
			},
			//词库文件上传文件成功触发的方法 end
			//词库文件上传文件成功失败的方法 start
			headimageFail(e) {
				console.log('上传文件失败', e);
			},
			//词库文件上传文件成功失败的方法 end
			//词库文件列表移除时触发的方法 start
			headimageDelete(e) {
				console.log('删除文件从列表', e);
				
			},
			//词库文件列表移除时触发的方法 end
			//词库文件上传时触发的方法 start
			headimageProgress(e) {
				console.log('上传文件进度', e);
			},
			//词库文件上传时触发的方法 end
			//日期选择器监听事件 start
			dateChange(e){
				console.log(e);
				console.log(this.user.birthday);
				let nowYear=parseInt(this.globalData.getDateTime(20));
				let chooseDate = new Date(Date.parse(e.replace(/-/g, "/")));
				let chooseyear = chooseDate.getFullYear();
				console.log(nowYear,chooseyear);
				if(nowYear<chooseyear){
					this.user.age=''
					this.$refs['user'].setValue('age','' );
				}else{
					this.user.age=nowYear-chooseyear;
					this.$refs['user'].setValue('age',this.user.age );
				}
			},
			//日期选择器监听事件 end
			//载入图片错误 start
			imageError(e){
				this.headimageUrl=this.globalData.defaultHeadImage;
			},
			//载入图片错误 end
			// 点击图像 start
			atvarClick() {	
				
					
			},
			//点击图像 end
			//加载数据 start
			loadData(){
				this.mainStyle.backgroundImage=`url("${this.globalData.backgroundUrl2}")`;
				this.loadNetData();
			},
			//加载数据 end
			//加载服务器数据 start
			loadNetData(){
				if(this.user.id!=0){
					uni.request({
						url:this.globalData.url+"user/getUser",
						dataType:'json',
						method:'POST',
						header:{
							'content-type':this.globalData.contentType.normalData,
							'token':uni.getStorageSync('token')
						},
						data:{
							id:this.user.id
						},
						success: (res) => {
							
							let data = res.data;
							if(data.code==200){
							
								
									
								this.user =data.data;
				
								this.headimageUrl=this.globalData.url+'static/image/headImage/'+this.user.headimage;
								
							}else{
								uni.clearStorage();
								this.globalData.checkLogin();
							}
						},
						fail: (e) => {
							console.log(e);
							uni.clearStorage();
							this.globalData.checkLogin();
						}
					})
				}
			
			},
			//加载服务器数据 end
		}
	}
</script>

<style>
	.header,.body,.footer{
		display: flex;
		justify-content: center;
		text-align: center;
	}
 .header{

}
.body{

}
.content{

}
.footer{

}

</style>
