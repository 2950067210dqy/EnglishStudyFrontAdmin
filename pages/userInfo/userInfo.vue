<template>
	<view class="main" :style="mainStyle">
		<my-navBack backColor="white"  background="linear-gradient(90deg, #0d0d0d, #595959)" title="个人信息"></my-navBack>
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
							disabled
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
							disabled
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
							disabled
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
							disabled
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
			<view class="login" style="margin-right: 30rpx;"><button @tap="exitLogin" class="button1" type="normal">退出登录</button></view>
			<view class="login" style="margin-right: 30rpx;" ><button @tap="toNormalUser" style="background:  linear-gradient(90deg, #00aaff, #00e66f);color: white;" class="button1" type="normal">撤销管理员权限</button></view>
			<view class="login" style="margin-right: 30rpx;"> <navigator url="../forgetPwd/forgetPwd" open-type="navigate"> <button class="button1" type="primary">修改密码</button></navigator> </view>
			<view class="login"><button @tap="update" class="button1" type="warn">修改信息</button></view>
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
		onLoad() {
			this.globalData=getApp().globalData;
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
			//降为普通用户 start
			toNormalUser(){
				if(this.user.type!=1){
					if(this.user.type==0){
						uni.showToast({
							title:"权限不够！",
							icon:'error'
						})
					}
					if(this.user.type==-1){
						uni.showToast({
							title:"超级管理员不能变成用户！",
							icon:'error'
						})
					}
					
					return;
				}
				uni.showModal({
					title:"注意",
					content:"您确定要变成用户吗？",
					success: (res) => {
						if(res.confirm){
							uni.request({
								url:this.globalData.url+"adminUser/toNormalUser",
								dataType:'json',
								method:'POST',
								header:{
									'content-type':this.globalData.contentType.normalData,
									'token':uni.getStorageSync('token')
								},
								data:{
									id :this.user.id,
								},
								success: (res) => {
									let data = res.data;
									if(data.code==200){
										uni.showToast({
											title:"撤销成功"
										});
										uni.clearStorage();
										this.globalData.checkLogin();
									}else{
										uni.showToast({
											title:"撤销失败"
										});
										console.log(data.message);
									}
								},
								fail: (e) => {
									console.log(e);
									
								}
							})
							
							
						
						}
					}
				})
			},
			//降为普通用户 end
			//退出登录 start
			exitLogin(){
				uni.showModal({
					title:"注意",
					content:"您确定要退出登录吗？",
					success: (res) => {
						if(res.confirm){
							uni.showToast({
								title:"成功退出"
							})
							uni.clearStorage();
							this.globalData.checkLogin();
						}
					}
				})
			
			},
			//退出登录 end
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
							if(data.data.id!=JSON.parse(uni.getStorageSync('user')).id){
								uni.clearStorage();
								console.log("@");
								this.globalData.checkLogin();
							}else{
									uni.showToast({
										title:"修改成功"
									})
									this.user =data.data;
									uni.setStorageSync("user",JSON.stringify(data.data));
									this.headimageUrl=this.globalData.url+'static/image/headImage/'+this.user.headimage;
							}
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
				uni.request({
					url:this.globalData.url+"user/getUser",
					dataType:'json',
					method:'POST',
					header:{
						'content-type':this.globalData.contentType.normalData,
						'token':uni.getStorageSync('token')
					},
					data:{
						id:JSON.parse(uni.getStorageSync('user')).id
					},
					success: (res) => {
						
						let data = res.data;
						if(data.code==200){
						
							if(data.data.id!=JSON.parse(uni.getStorageSync('user')).id){
								uni.clearStorage();
								console.log("@");
								this.globalData.checkLogin();
							}else{
								
									this.user =data.data;
									uni.setStorageSync("user",JSON.stringify(data.data)  );
									this.headimageUrl=this.globalData.url+'static/image/headImage/'+this.user.headimage;
							}
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
