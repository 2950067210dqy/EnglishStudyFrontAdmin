<template>
	<view class="main">
		<my-navBack backColor="white"  background="linear-gradient(90deg, #0d0d0d, #595959)" :title="mode=='insert'?'添加题目':'修改题目'"></my-navBack>
		<view class="header">	
			
			
		</view>
		<view class="body">
			<view class="content" style="padding: 50rpx;"> 
				<uni-forms validate-trigger="bind" :rules="testRules"  ref="testDetail" :modelValue="tests" labelWidth="150">
					<!-- 	<uni-forms-item   name="cikuTypeId">
							<uni-easyinput   type="hidden" v-model="formData.selectCikuType.cikuTypeId"  />
						</uni-forms-item> -->
					<uni-forms-item  required label="题目" name="title">
				
						<uni-easyinput
							:focus="titleFocus"
							@blur="binddata('title', tests.title, 'testDetail');titleFocus=false"
							:trim="true"
							type="textarea"
							textAreaStyle="height:800rpx;font-size:33rpx;line-height:70rpx;-webkit-box-orient: vertical;word-break: break-all;;word-wrap: break-word;
							word-spacing:3rpx;padding: 20rpx;text-indent: 1rem;width:100%"
							placeholder="请输入题目"
							v-model="tests.title"
						>
							<template v-slot:code>
								<view style="padding: 20rpx;">
									<button type="primary" size="mini" @click="insertNotation">添加问题符号( _____ )</button>
								</view>
							</template>
						</uni-easyinput>
					</uni-forms-item>
					<uni-forms-item  required label="选项A" name="opta">
						<uni-easyinput
						@blur="binddata('opta', tests.opta, 'testDetail')"
							:trim="true"
							type="text"
							placeholder="请输入选项A"
							v-model="tests.opta"
						>
							
						</uni-easyinput>
					</uni-forms-item>
					<uni-forms-item  required label="选项B" name="optb">
						<uni-easyinput
						@blur="binddata('optb', tests.optb, 'testDetail')"
							:trim="true"
							type="text"
							placeholder="请输入选项B"
							v-model="tests.optb"
						>
							
						</uni-easyinput>
					</uni-forms-item>
					<uni-forms-item  required label="选项C" name="optc">
						<uni-easyinput
						@blur="binddata('optc', tests.optc, 'testDetail')"
							:trim="true"
							type="text"
							placeholder="请输入选项C"
							v-model="tests.optc"
						>
							
						</uni-easyinput>
					</uni-forms-item>
					<uni-forms-item  required label="选项D" name="optd">
						<uni-easyinput
						@blur="binddata('optd', tests.optd, 'testDetail')"
							:trim="true"
							type="text"
							placeholder="请输入选项D"
							v-model="tests.optd"
						>
							
						</uni-easyinput>
					</uni-forms-item>
					<uni-forms-item  required label="答案" name="answer">
						<uni-data-checkbox
						@blur="binddata('answer', tests.answer,'testDetail')"
							:check="1"
							title="选项:"
							titleColor="black"
							radioSize="35rpx"
							radioFontSize="35rpx"
							titleFontSize="35rpx"
							v-model="tests.answer"
							:localdata="answer"
						></uni-data-checkbox>
					</uni-forms-item>
					<uni-forms-item   label="题目解析" >
						
						<uni-easyinput
							
							textAreaStyle="height:800rpx;font-size:33rpx;line-height:70rpx;-webkit-box-orient: vertical;word-break: break-all;;word-wrap: break-word;
		word-spacing:3rpx;padding: 20rpx;text-indent: 1rem;width:100%"
							type="textarea"
							
							placeholder="请输入解析"
							:maxlength="-1"
							v-model="tests.analy"
						>
							
						</uni-easyinput>
					</uni-forms-item>
				</uni-forms>
				<button type="default"  @click="updateTest" v-if="mode=='update'" style="color: white;background-color: rgb(199, 133, 0);margin-top: 40rpx;">修改题目</button>
				<button type="default" @click="insertTest"  v-else style="color: white;background-color: rgb(199, 133, 0);margin-top: 40rpx;">添加题目</button>
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
				testType:undefined,
				current:undefined,
				mode:"insert",
				titleFocus:true,
				answer: [{
									text: 'A',
									value: 0
				
								}, {
									text: 'B',
									value: 1,
				
								}, {
									text: 'C',
									value: 2
								}, {
									text: 'D',
									value: 3
								}
								
								],
				
				tests:{
					title:'',
					opta:'',
					optb:'',
					optc:'',
					optd:'',
					answer:0,
					analy:''
				},
				testsBackUp:{
					title:'',
					opta:'',
					optb:'',
					optc:'',
					optd:'',
					answer:0,
					analy:''
				},
				testRules: {
					title: {
						label: '题目标题内容',
						rules: [
							{
								required: true,
								errorMessage: '{label}不能为空，请填写'
							},
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
								validateFunction: function(rule, value, data, callback) {}
							}
						]
					},
					opta: {
						label: '选项A',
						rules: [
							{
								required: true,
								errorMessage: '{label}不能为空，请填写'
							},
							// 对name字段进行长度验证
							{
								minLength: 1,
								maxLength: 500,
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
					optd: {
						label: '选项D',
						rules: [
							{
								required: true,
								errorMessage: '{label}不能为空，请填写'
							},
							// 对name字段进行长度验证
							{
								minLength: 1,
								maxLength: 500,
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
					optb: {
						label: '选项B',
						rules: [
							{
								required: true,
								errorMessage: '{label}不能为空，请填写'
							},
							// 对name字段进行长度验证
							{
								minLength: 1,
								maxLength: 500,
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
					optc: {
						label: '选项C',
						rules: [
							{
								required: true,
								errorMessage: '{label}不能为空，请填写'
							},
							// 对name字段进行长度验证
							{
								minLength: 1,
								maxLength: 500,
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
					answer: {
						label: '文章简介',
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
			if(param.testType!=undefined){
				this.testType=parseInt(param.testType); 
			}
			
			if(param.current!=undefined){
				this.current = parseInt(param.current);
			}
			
			
		},
		onShow() {
			this.globalData=getApp().globalData;
			if(this.testType==undefined){
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
			uni.setStorageSync("testType",this.testType);
			
			uni.setStorageSync("testCurrent",this.current);
			if(this.id!=undefined||this.id==0){
				this.mode="update";
			}else{
				this.mode="insert";
			}
			
			if(this.globalData.checkLogin()){
				this.loadData();
			}
		},
		onTesty() {
			this.$refs['testDetail'].setRules(this.testRules);
		},
		methods: {
			//添加符号 start
			insertNotation(){
				this.tests.title+="  ______  ";
				this.$nextTick(() => {					
						this.titleFocus=true;        	
				})
			
			},
			//添加符号 end
			//添加题目 start
			insertTest(){
				this.$refs['testDetail']
					.validate()
					.then(res => {
						console.log('表单数据信息：', res);
						
						// 使用 Promise then/catch 方式调用
				
						uni.request({
							url:this.globalData.url+"adminTest/insert",
							method:'POST',
							header:this.globalData.header.normal,
							dataType:"json",
							data:{
								test:JSON.stringify(this.tests),
								testType:this.testType,
							
							}
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								uni.showToast({
									title:"添加成功"
								})
								this.tests=JSON.parse(JSON.stringify(this.testsBackUp));
								
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
			//添加题目 end
			
			//修改题目 start
			updateTest(){
				
				uni.request({
					url:this.globalData.url+"adminTest/update",
					method:'POST',
					header:this.globalData.header.normal,
					dataType:"json",
					data:{
						test:JSON.stringify(this.tests),
						testType:this.testType,
					
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
			//修改题目 end
			
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
							url:this.globalData.url+"adminTest/getOneById",
							method:'POST',
							header:this.globalData.header.normal,
							dataType:"json",
							data:{
								testType:this.testType,
							
								id:this.id
							}
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.tests = data.data;
							
								this.testsBackUp = JSON.parse(JSON.stringify(this.tests));
								
								

								console.log(this.tests);
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
