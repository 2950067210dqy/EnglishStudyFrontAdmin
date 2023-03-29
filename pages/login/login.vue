<template>

	<view class="main" :style="mainStyle" style="height: 100%;">
		

		<view class="content">
			<view class="body">
				<image src="../../static/logo1.png" style="width: 250rpx;height:250rpx;border-radius: 50%;"></image>
				<view style="color: white;font-size: 47rpx;font-weight: bolder;margin-top: 30rpx;">沁柚学习平台后台登录</view>
				
				<template v-if="current == 0">
					<uni-forms ref="loginFormUserName" :rules="formUserNameRules" :modelValue="formDataUserName">
						<view class="header">账号密码登录</view>
						<uni-forms-item name="username">
							<uni-easyinput
								color="white"
								fontSize="35rpx"
								prefixIcon="contact"
								height="90rpx"
								:clearSize="20"
								:trim="true"
								:maxlength="20"
								type="text"
								v-model="formDataUserName.username"
								placeholder="请输入账号"
								placeholderStyle="color:rgb(234, 234, 234);font-size:35rpx"
							/>
						</uni-forms-item>
						<uni-forms-item name="password">
							<uni-easyinput
								color="white"
								fontSize="35rpx"
								prefixIcon="eye"
								height="90rpx"
								:trim="true"
								:maxlength="50"
								type="password"
								v-model="formDataUserName.password"
								placeholder="请输入密码"
								placeholderStyle="color:rgb(234, 234, 234);font-size:35rpx"
							/>
						</uni-forms-item>
					</uni-forms>
				</template>
				<template v-if="current == 1">
					<uni-forms ref="loginFormPhone" :rules="formPhoneRules" :modelValue="formDataPhone">
						<view class="header">手机号验证登录</view>
						<uni-forms-item name="phone">
							<uni-easyinput
							 @blur="binddata('phone', formDataPhone.phone,'loginFormPhone')"
								color="white"
								fontSize="35rpx"
								prefixIcon="phone-filled"
								height="90rpx"
								:clearSize="20"
								:trim="true"
								:maxlength="20"
								type="number"
								v-model="formDataPhone.phone"
								placeholder="请输入手机号码"
								placeholderStyle="color:rgb(234, 234, 234);font-size:35rpx"
							/>
						</uni-forms-item>
						<uni-forms-item name="code">
							<uni-easyinput
							@blur="binddata('code', formDataPhone.code,'loginFormPhone')"
								color="white"
								fontSize="35rpx"
								prefixIcon="eye"
								height="90rpx"
								:trim="true"
								:maxlength="50"
								type="text"
								v-model="formDataPhone.code"
								placeholder="请输入验证码"
								placeholderStyle="color:rgb(234, 234, 234);font-size:35rpx"
							>
								<template v-slot:code>
									<view style="text-align: center;">
										<button :disabled="codeDisabled||codeDisabled0" @tap="getCode" type="default" style="display: inline-block;text-align: center;font-size: 25rpx;">
											<text style="text-align: center;">{{ codeText }}</text>
										</button>
									</view>
								</template>
							</uni-easyinput>
						</uni-forms-item>
					</uni-forms>
				</template>

				<view class="middle">
					<view class="middleRadio">
						<checkbox-group style="display: inline-block;" @change="clickReadXieyi"><checkbox :checked="xieYiState" /></checkbox-group>

						<view style="display: inline-block;">
							我已阅读并同意
							<text class="xieyi1" @tap="showXieyi1">《隐私协议》</text>
							和
							<text class="xieyi2" @tap="showXieyi2">《服务条款》</text>
						</view>
					</view>
				</view>
				<view class="login"><button @tap="login" class="button1" type="normal">登录</button></view>

				
			</view>
			<view class="footer">
				<view>其他登陆方式</view>

				<view class="loginType">
					<view v-if="current == 0" class="type">
						<button style="border: none;" shape="circle" type="default" :plain="true" link="true" open-type="getPhoneNumber" @tap="choosePhoneLogin">
							<uni-icons type="phone-filled" color="rgb(232, 232, 232)" size="40"></uni-icons>
						</button>
					</view>
					<view v-if="current == 1" class="type">
						<button style="border: none;" shape="circle" type="default" :plain="true" link="true" open-type="getPhoneNumber" @tap="chooseUserNameLogin">
							<uni-icons type="contact" color="rgb(232, 232, 232)" size="40"></uni-icons>
						</button>
					</view>
				
					/* #ifdef MP-WEIXIN */
					<view class="type">
						<button style="border: none;" shape="circle" type="default" :plain="true" link="true" @tap="wxLogin">
							<uni-icons type="weixin" color="rgb(232, 232, 232)" size="40"></uni-icons>
						</button>
					</view>
					/*  #endif  */
					/* #ifdef MP-QQ */
					<view class="type">
						<button style="border: none;" shape="circle" type="default" :plain="true" link="true"  @tap="qqLogin" >
							<uni-icons type="qq" color="rgb(232, 232, 232)" size="40"></uni-icons>
						</button>
					</view>
					/*  #endif  */
				</view>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			current: 0,
			xieYiState: true,
			codeText: '发送验证码',
			codeDisabled0:true,
			codeDisabled: false,
			session: '',
			formDataUserName: {
				username: 'admin',
				password: 'admin'
			},
			formDataPhone: {
				phone: '',
				code: ''
			},
			formPhoneRules: {
				phone: {
					label: '手机号码',
					rules: [
						{
							required: true,
							errorMessage: '{label}不能为空，请填写'
						},
						{
							format: 'string',
							errorMessage: '{label}不能出现其他字符，只为数字'
						},
						{
							validateFunction: (rule, value, data, callback)=> {
								let minLength = 11;
								let maxLength = 11;
								console.log(value,typeof value);
								if (value.length == 0) {
									this.codeDisabled0=true;
									callback('{label}不能为空，请填写');
								} else if (value.length > maxLength || value.length < minLength) {
									this.codeDisabled0=true;
									callback('{label}长度应该在' + minLength + '位');
								} else {
									this.codeDisabled0=false;
									callback();
								
								}
							}
						}
					]
				},
				code: {
					label: '验证码',
					rules: [
						{
							require: true,
							errorMessage: '{label}不能为空，请填写'
						},
						{
							format: 'string',
							errorMessage: '{label}只为字符'
						},
						// 对name字段进行长度验证
						{
							validateFunction: function(rule, value, data, callback) {
								let minLength = 6;
								let maxLength = 6;
								if (value.trim().length == 0) {
									callback('{label}不能为空，请填写');
								} else if (value.trim().length > maxLength || value.trim().length < minLength) {
									callback('{label}长度应该在' + minLength + '位');
								} else {
									callback();
								}
							}
						}
					]
				}
			},
			formUserNameRules: {},
			mainStyle: {
				backgroundImage: 'url("../../static/bg2.jpg")',
				// backgroundColor: 'black',
				backgroundRepeat: 'no-repeat',
				backgroundSize: 'cover',
				position: 'relative'
			}
		};
	},
	onLoad() {
		this.globalData = getApp().globalData;
	},
	onShow() {
		
		this.globalData = getApp().globalData;
		console.log(123);
		this.mainStyle.backgroundImage=`url("${this.globalData.backgroundUrl2}")`;
		
	},
	updated() {
	
		// 需要在onReady中设置规则
		
		if (this.current == 1) {
			this.$refs['loginFormPhone'].setRules(this.formPhoneRules);
		} else {
			this.$refs['loginFormUserName'].setRules(this.formUserNameRules);
		}
	},
	onReady() {
		// 需要在onReady中设置规则
					
					if (this.current == 1) {
						this.$refs['loginFormPhone'].setRules(this.formPhoneRules);
					} else {
						this.$refs['loginFormUserName'].setRules(this.formUserNameRules);
					}
			
	
	},
	methods: {
		//协议 start
		showXieyi1(){
			uni.showModal({
				title:'沁柚隐私协议',
				content:'隐私协议请不要违反',
				showCancel:false
			});
		},
		showXieyi2(){
			uni.showModal({
				title:'沁柚服务条款',
				content:'服务条款请遵守',
				showCancel:false
			});
		},
		//协议 end
		//微信一键登录 start
		wxLogin(e){
			uni.showLoading({title:'登陆中'});
			uni.login({
				provider:"weixin",
				success: (res) => {
					if(res.errMsg=="login:ok"){
						uni.request({
							url:this.globalData.url+"adminUser/wxloginAdmin",
							dataType:"json",
							method:'POST',
							header:this.globalData.contentType.normal,
							data:{
								code:res.code
							},
							success:(res)=>{
								let data =res.data;
								if(data.code==200){
									console.log(data);
									uni.setStorageSync('user', JSON.stringify(data.data.user));
									uni.setStorageSync('token', data.data.token);
									uni.hideLoading();
									uni.reLaunch({
										url:"/pages/index/index"
									});
								}else{
									uni.hideLoading();
									uni.showToast({title:'登陆失败4'})
								}
							},
							fail:(e)=>{
								console.log(e);
								uni.hideLoading();
								uni.showToast({title:'登陆失败3'})
							}
						})
					}else{
						wx.hideLoading();
						wx.showToast({title:'登陆失败1'})
					}
					
				},fail: (e) => {
					console.log(e);
					wx.hideLoading();
					wx.showToast({title:'登陆失败2'})
				}
			})
			
		
							
		},
		//微信一键登录 end
		//qq一键登录 start
		qqLogin(e){
			uni.showLoading({title:'登陆中'});
			uni.login({
				provider:"qq",
				success: (res) => {
					if(res.errMsg=="login:ok"){
						uni.request({
							url:this.globalData.url+"adminUser/qqlogin",
							dataType:"json",
							method:'POST',
							header:this.globalData.contentType.normal,
							data:{
								code:res.code
							},
							success:(res)=>{
								let data =res.data;
								if(data.code==200){
									console.log(data);
									uni.setStorageSync('user', JSON.stringify(data.data.user));
									uni.setStorageSync('token', data.data.token);
									uni.hideLoading();
									uni.reLaunch({
										url:"/pages/index/index"
									});
								}else{
									uni.hideLoading();
									uni.showToast({title:'登陆失败4'})
								}
							},
							fail:(e)=>{
								console.log(e);
								uni.hideLoading();
								uni.showToast({title:'登陆失败3'})
							}
						})
					}else{
						wx.hideLoading();
						wx.showToast({title:'登陆失败1'})
					}
					
				},fail: (e) => {
					console.log(e);
					wx.hideLoading();
					wx.showToast({title:'登陆失败2'})
				}
			})
			
		
							
		},
		//qq一键登录 end
		//点击阅读了协议checkbox start
		clickReadXieyi() {
			this.xieYiState = !this.xieYiState;
		},
		//点击阅读了协议checkbox end
		//查看是否阅读协议条款 start
		checkXieyi() {
			if (!this.xieYiState) {
				uni.showModal({
					title: '提示',
					content: '请勾选我已阅读协议和条款，点击确定自动勾选',
					success: res => {
						if (res.confirm) {
							this.xieYiState = true;
							return true;
						} else if (res.cancel) {
							return false;
						}
					}
				});
			} else {
				return true;
			}
		},
		//查看是否阅读协议条款 end
		//登录 start
		login() {
			if (this.current == 1) {
				this.$refs['loginFormPhone']
					.validate()
					.then(res => {
						if (this.checkXieyi()) {
							uni.showLoading({
								title: '正在登录'
							});
							uni.request({
								url: this.globalData.url + 'code/verifyCode',
								method: 'POST',
								dataType: 'json',
								data: {
									phone: this.formDataPhone.phone,
									code: this.formDataPhone.code
								},
								xhrFields: { withCredentials: true },
								header: {
									'content-type': this.globalData.contentType.jsonData,
									cookie: this.session
								},

								success: res => {
									let data = res.data;
									if (data.code == 200) {
										uni.request({
											url: this.globalData.url + 'adminUser/loginByPhone',
											method: 'POST',
											dataType: 'json',
											data: {
												phone: this.formDataPhone.phone
											},
											header: this.globalData.contentType.normal,
											success: res => {
												let data = res.data;
												if (data.code == 200) {
													console.log(data);
													uni.setStorageSync('user', JSON.stringify(data.data.user));
													uni.setStorageSync('token', data.data.token);
													uni.hideLoading();
													uni.reLaunch({
														url:"/pages/index/index"
													});
												} else {
													uni.hideLoading();
													uni.showToast({
														title: data.message
													});
												}
											},
											fail: e => {
												uni.hideLoading();
												uni.showToast({
													title: e.toString()
												});
											}
										});
									} else {
										uni.hideLoading();
										uni.showToast({
											title: data.message
										});
									}
									// console.log(res);
								},
								fail: e => {
									uni.hideLoading();
									uni.showToast({
										title: e.toString()
									});
								}
							});
							console.log('表单数据信息：', res);
						}
					})
					.catch(err => {
						uni.showToast({
							icon: 'error',
							title: '数据格式错误'
						});
						console.log('表单错误信息：', err);
					});
			} else {
				this.$refs['loginFormUserName']
					.validate()
					.then(res => {
						if (this.checkXieyi()) {
							uni.request({
								url: this.globalData.url + 'adminUser/loginByUserName',
								method: 'POST',
								dataType: 'json',
								data: {
									username: this.formDataUserName.username,
									password: this.formDataUserName.password
								},
								header: this.globalData.contentType.normal,
								success: res => {
									let data = res.data;
									if (data.code == 200) {
										uni.setStorageSync('user', JSON.stringify(data.data.user));
										uni.setStorageSync('token', data.data.token);
										uni.hideLoading();
										uni.reLaunch({
											url:"/pages/index/index"
										});
									} else {
										uni.hideLoading();
										uni.showToast({
											title: data.message
										});
									}
								},
								fail: e => {
									uni.hideLoading();
									uni.showToast({
										title: e.toString()
									});
								}
							});
							console.log('表单数据信息：', res);
						}
					})
					.catch(err => {
						uni.showToast({
							icon: 'error',
							title: '数据格式错误'
						});
						console.log('表单错误信息：', err);
					});
			}
		},
		//登录 end
		//切换手机登录 start
		choosePhoneLogin() {
			this.current = !this.current;
		},
		//切换手机登录 end
		//切换用户名密码登录 start
		chooseUserNameLogin() {
			this.current = !this.current;
		},
		//切换用户名密码登录 end
		//发送验证码 start
		getCode(e) {
			// 部分表单进行校验，接受一个参数，类型为 String 或 Array ，只校验传入 name 表单域的值
			this.$refs['loginFormPhone']
				.validateField(['phone'])
				.then(res => {
					// 成功返回，res 为对应表单数据
					// 其他逻辑处理
					// ...
					if (this.checkXieyi()) {
						this.codeText = '60';
						this.codeDisabled = true;
						let timer = setInterval(() => {
							this.codeText = (parseInt(this.codeText) - 1).toString();
							if (this.codeText.length == 1) {
								this.codeText = '0' + this.codeText;
							}
							if (this.codeText == '00') {
								clearInterval(timer);
								this.codeText = '发送验证码';
								this.codeDisabled = false;
							}
						}, 1000);

						uni.request({
							url: this.globalData.url + 'code/getcode',
							method: 'POST',
							dataType: 'json',
							data: {
								phone: this.formDataPhone.phone,
								code: ''
							},
							xhrFields: { withCredentials: true },
							header: this.globalData.contentType.json,
							success: res => {
								let data = res.data;
								this.session = res.cookies[0];
								console.log(this.session);
								if (data.code == 200) {
									uni.showToast({
										title: '发送成功'
									});
								} else {
									uni.showToast({
										title: data.message
									});
								}
							},
							fail: e => {
								uni.showToast({
									title: e.toString()
								});
							}
						});
					}
				})
				.catch(err => {
					// 表单校验验失败，err 为具体错误信息
					// 其他逻辑处理
					// ...
				});
		}
		//发送验证码 end
	}
};
</script>

<style>
.main {
	height: 100vh;
}
.content {
	display: flex;
	text-align: center;
	justify-content: center;
	flex: 1;
	flex-wrap: nowrap;
	flex-direction: column;
}
.header,
.body,
.footer {
}
.header {
	font-size: 50rpx;
	color: white;
	margin-top: 40rpx;
	text-align: left;
	margin-bottom: 30rpx;
}
.headerImage {
	width: 220rpx;
	height: 220rpx;
	border-radius: 10rpx;
	box-shadow: 2px 2px 1px grey;
}
.body,
.footer {
	padding-left: 60rpx;
	padding-right: 60rpx;
	padding-top: 60rpx;
	/* background-color: aliceblue; */
}
.middle {
	width: 100vw;
	color: white;
}
.middleRadio {
	text-align: left;
	justify-self: left;
}
.xieyi1,
.xieyi2 {
	font-size: 30rpx;
	font-weight: bolder;
	color: rgb(234, 234, 234);
}
.login {
	margin-top: 30rpx;
}
.info {
	margin-top: 20rpx;
	color: white;
	font-weight: normal;
}
.text1 {
	float: left;
}

.text2 {
	float: right;
}
.text1:hover,
.text2:hover {
	/* border-bottom: 1px solid rgb(11, 130, 132); */
	color: rgb(11, 130, 132);
}
.footer {
	font-weight: bolder;
	font-size: 32rpx;
	margin-top: 50rpx;
	color: white;
}
.loginType {
	margin-top: 60rpx;
}
.type {
	display: inline-block;
	margin-left: 40rpx;
}
.button1 {
	border-radius: 10rpx;
	box-shadow: 2rpx 2rpx 2rpx whitesmoke;
	background-color: rgb(146, 59, 75);
	color: white;
}
.button1:active {
	background-color: rgb(91, 36, 47);
	color: rgb(162, 215, 162);
}
</style>
