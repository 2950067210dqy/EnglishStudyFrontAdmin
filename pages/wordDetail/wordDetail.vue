<template>
	<view class="main" style="position: relative;">
		<my-navBack  backColor="white"  background="linear-gradient(90deg, #0d0d0d, #595959)" :title="mode=='insert'?'添加单词':'修改单词'"></my-navBack>
		<view class="header">	
			
			
		</view>
		<view class="body" :style="`width: ${flexWidth}vw;`">
			<view class="content" style="padding: 50rpx;"> 
				<uni-forms validate-trigger="bind"  ref="wordDetail" :modelValue="words" labelWidth="150">
					<!-- 	<uni-forms-item   name="cikuTypeId">
							<uni-easyinput   type="hidden" v-model="formData.selectCikuType.cikuTypeId"  />
						</uni-forms-item> -->
					<uni-forms-item  required label="单词/词组" name="word">

						<uni-easyinput
							@blur="binddata('word', words.word.word, 'wordDetail')"
							:trim="true"
							type="text"
							placeholder="请输入单词/词组"
							v-model="words.word.word"
						>
							<template v-slot:code>
								<view style="text-align: center;" v-if="!showYoudao">
									<button  @tap="toggleYouDao" type="warn" style="display: inline-block;text-align: center;font-size: 20rpx;">
										<text style="text-align: center;">打开有道翻译网页</text>
									</button>
								</view>
							</template>
						</uni-easyinput>
					</uni-forms-item>
					<uni-forms-item  required label="词性" name="charac">
						<uni-easyinput
						@blur="binddata('charac', words.word.charac, 'wordDetail')"
							:trim="true"
							type="text"
							placeholder="请输入词性"
							v-model="words.word.charac"
						>
							
						</uni-easyinput>
					</uni-forms-item>
					<uni-forms-item  required label="译文" name="trans">
						<uni-easyinput
						@blur="binddata('trans', words.word.trans, 'wordDetail')"
							:trim="true"
							type="textarea"
							placeholder="请输入译文"
							v-model="words.word.trans"
						>
							
						</uni-easyinput>
					</uni-forms-item>
					<uni-forms-item   label="英式音标" name="soundmark1">
						<uni-easyinput
							:trim="true"
							type="textarea"
							placeholder="请输入英式音标"
							v-model="words.word.soundmark1"
						>
							<template v-slot:code >
								<view v-if="audioWordUKFileExist"  style="color: rgb(0, 214, 0);margin-right: 10rpx;border: rgb(0, 214, 0) 5rpx solid;padding: 20rpx;font-weight: bold;">
									存在音频文件
									<uni-icons color="rgb(0, 214, 0)" type="checkmarkempty" size="16"></uni-icons>
									<mp-audio
										:src="`${globalData.url}static/audio/1/${words.word.word.slice(0,1).toLowerCase()}/${words.word.word}.mp3`"
										:poster="globalData.defaultHeadImage"
										:name="words.word.word"
										author="英式发音"
										:controls="true"
									></mp-audio>
									<button type="warn" @tap="audioUKFileDelete" style="margin-top: 20rpx;font-size: 30rpx;">删除音频文件</button>
								</view>
								<view v-else style="color: rgb(221, 0, 0);margin-right: 10rpx;border: rgb(221, 0, 0) 5rpx solid;padding: 20rpx;font-weight: bold;">
									不存在音频文件
									<uni-icons color="rgb(221, 0, 0)" type="closeempty" size="16"></uni-icons>
								</view>
								
								<view style="text-align: center; " v-if="words.word.word!=undefined&&words.word.word.trim()!=''&&words.word.word.trim().length!=0&&!wordExist">
									上传音频文件
									<uni-file-picker
										@select="audioUKFileSelect"
										@delete="audioUKFileDelete"
										ref="importFileByCiku"
										limit="1"
										file-mediatype="all"
										title="最多选择1个文件"
										fileExtname="mp3"
										:modelValue="[]"
									></uni-file-picker>
								</view>
							</template>
						</uni-easyinput>
					</uni-forms-item>
					<uni-forms-item   label="美式音标" name="soundmark2">
						<uni-easyinput
							:trim="true"
							type="textarea"
							placeholder="请输入美式音标"
							v-model="words.word.soundmark2"
						>
							<template v-slot:code >
								<view v-if="audioWordUSAFileExist"  style="color: rgb(0, 214, 0);margin-right: 10rpx;border: rgb(0, 214, 0) 5rpx solid;padding: 20rpx;font-weight: bold;">
									存在音频文件
									<uni-icons color="rgb(0, 214, 0)" type="checkmarkempty" size="16"></uni-icons>
									<mp-audio
										:src="`${globalData.url}static/audio/2/${words.word.word.slice(0,1).toLowerCase()}/${words.word.word}.mp3`"
										:poster="globalData.defaultHeadImage"
										:name="words.word.word"
										author="美式发音"
										:controls="true"
									></mp-audio>
									<button type="warn" @tap="audioUSAFileDelete" style="margin-top: 20rpx;font-size: 30rpx;">删除音频文件</button>
								</view>
								<view v-else style="color: rgb(221, 0, 0);margin-right: 10rpx;border: rgb(221, 0, 0) 5rpx solid;padding: 20rpx;font-weight: bold;">
									不存在音频文件
									<uni-icons color="rgb(221, 0, 0)" type="closeempty" size="16"></uni-icons>
								</view>
								<view style="text-align: center;" v-if="words.word.word!=undefined&&words.word.word.trim()!=''&&words.word.word.trim().length!=0&&!wordExist">
									上传音频文件
									<uni-file-picker
										@select="audioUSAFileSelect"
										@delete="audioUSAFileDelete"
										ref="importFileByCiku"
										limit="1"
										file-mediatype="all"
										title="最多选择1个文件"
										fileExtname="mp3"
										:modelValue="[]"
									></uni-file-picker>
									
								</view>
							</template>
						</uni-easyinput>
					</uni-forms-item>
					<button type="primary" @tap="addLiju" style="margin-top: 20rpx;margin-bottom: 20rpx;">添加例句</button>
					<swiper :current="lijuCurrent" :indicator-dots="true"  style="height: 950rpx;padding: 20rpx;border: 5rpx dashed grey;">
						<template v-for="(item,index) in words.liju.sentences" :key="index">
							<swiper-item >
								<uni-forms-item   :label="'例句'+(index+1)" name="liju">
									<view >
										<view style="margin-bottom: 20rpx;">
											例句英语
										</view>
										<uni-easyinput
											:trim="true"
											type="textarea"
											placeholder="请输入例句英语"
											v-model="item.english"
										>
											
										</uni-easyinput>
										<view style="margin-bottom: 20rpx;margin-top: 20rpx;">
											例句译文
										</view>
										<uni-easyinput
											:trim="true"
											type="textarea"
											placeholder="请输入例句译文"
											v-model="item.chinese"
										>
											
										</uni-easyinput>
										<view style="margin-bottom: 20rpx;margin-top: 20rpx;">
											例句来源词典
										</view>
										<uni-easyinput
											:trim="true"
											type="textarea"
											placeholder="请输入例句来源词典"
											v-model="item.dict"
										>
											
										</uni-easyinput>
									</view>
									<button type="warn" @tap="deleteLiju(index)" style="margin-top: 20rpx;">删除例句</button>
								</uni-forms-item>
							</swiper-item>
							
						</template>
					</swiper>
						
					
					
				</uni-forms>
				<button type="default"  @click="updateWord" v-if="mode=='update'" style="color: white;background-color: rgb(199, 133, 0);margin-top: 40rpx;">修改单词</button>
				<button type="default" @click="insertWord"  v-else style="color: white;background-color: rgb(199, 133, 0);margin-top: 40rpx;">添加单词</button>
			</view>
		</view>
		
		
		<view style="position: absolute;top: 0;right: 0;" v-if="showYoudao">
			<button  @tap="toggleYouDao" type="warn" style="text-align: center;font-size: 37rpx;padding:13rpx;">
				<text style="text-align: center;">关闭有道翻译网页</text>
			</button>
			<web-view v-if="showYoudao"  :style="`width: 60vw;height: ${flexHeight}`" :fullscreen="false"  :src="`${words.word.word==undefined?globalData.youdaoUrlBase:globalData.youdaoUrl+words.word.word}`"></web-view>
		</view>
		<view class="footer" style="">
			
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				globalData:{},
				initial:undefined,
				id:undefined,
				current:undefined,
				mode:"insert",
				lijuCurrent:0,
				audioWordUKFileExist:false,
				audioWordUSAFileExist:false,
				audioWordUKFilePath:'',
				audioWordUSAFilePath:'',
				showYoudao:false,
				flexWidth:100,
				flexHeight:"95vh",
				wordExist:false,
				words:{
					word:{},
					liju:{
						sentences:[]
					}
				},
				wordsBackUp:{
					word:{},
					liju:{
						sentences:[]
					}
				},
				wordRules: {
					//cikuName 字段检验规则
					word: {
						label: '单词/词组',
						rules: [
							{
								required: true,
								errorMessage: '{label}不能为空，请填写'
							},
							// 对name字段进行长度验证
							{
								validateFunction: (rule, value, data, callback) => {
									
									// 异步需要返回 Promise 对象
									return new Promise((resolve, reject) => {
											value=value+'';
											
											if(value.length==0||value==''){
												reject(new Error('{label}不能为空'));
											}
										    let lengthvalidate =
											{
												minLength: 1,
												maxLength: 50,
												errorMessage: '{label}长度在 1 到 50 个字符'
											};
											if(value.trim().length<lengthvalidate.minLength||value.trim().length>lengthvalidate.maxLength){
												reject(new Error(lengthvalidate.errorMessage));
											}
											// console.log(data);
											uni.request({
												url: this.globalData.url + 'adminWord/getExist',
												method: 'POST',
												dataType: 'json',
												header: {
													'content-type': this.globalData.contentType.normalData,
													token: uni.getStorageSync('token')
												},
												data: {
													word:value.trim()
												},
												success: res => {
													let data = res.data;
													if (data.code == 200) {
														this.wordExist=true;
														reject(new Error(data.message));
													} else if (data.code == 520) {
														this.wordExist=false;
														this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
													} else {
														this.wordExist=false;
														this.loadWordAudioFileExists(1).then((res)=>{
															this.audioWordUKFileExist=res;		
															this.loadWordAudioFileExists(2).then((res)=>{
																this.audioWordUSAFileExist=res;	
															});
														});
														resolve();
													}
												},
												fail: error => {
													this.wordExist=false;
													reject(new Error('请求数据错误'));
												}
											});
										
									});
								}
							}
						]
					},
					charac: {
						label: '词性',
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
					trans: {
						label: '译文',
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
				},
				//
			}
		},
		onLoad(param) {
			this.globalData=getApp().globalData;
			this.initial = param.initial;
			if(param.id!=undefined){
				this.id =  parseInt(param.id);
			}
			
			if(param.current!=undefined){
				this.current = parseInt(param.current);
			}
			
			
		},
		onShow() {
			this.globalData=getApp().globalData;
			
			if(this.initial==undefined){
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
			uni.setStorageSync("initial",this.initial);
			uni.setStorageSync("wordCurrent",this.current);
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
			this.$refs['wordDetail'].setRules(this.wordRules);
		},
		methods: {
			//添加单词 start
			insertWord(){
				this.$refs['wordDetail']
					.validate()
					.then(res => {
						console.log('表单数据信息：', res);
						
						// 使用 Promise then/catch 方式调用
						this.words.liju.sentences = this.globalData.Base64.encode(JSON.stringify(this.words.liju.sentences));
						uni.request({
							url:this.globalData.url+"adminWord/insert",
							method:'POST',
							header:this.globalData.header.json,
							dataType:"json",
							data:this.words
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								uni.showToast({
									title:"添加成功"
								})
								this.words=JSON.parse(JSON.stringify(this.wordsBackUp));
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
			updateWord(){
				this.words.liju.sentences = this.globalData.Base64.encode(JSON.stringify(this.words.liju.sentences));
				uni.request({
					url:this.globalData.url+"adminWord/update",
					method:'POST',
					header:this.globalData.header.json,
					dataType:"json",
					data:this.words
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
			//删除文件 start
			deleteAudioFile(word,type){
				return new Promise((resolve,reject)=>{
					uni.showModal({
						title:"删除文件",
						content:"您确定要删除音频文件吗？",
						success: (res) => {
							if(res.confirm){
								uni.request({
									url:this.globalData.url+"adminFile/deleteAudio",
									method:'POST',
									header:this.globalData.header.normal,
									dataType:"json",
									data:{
										word:word,
										type:type
									}
								}).then((res)=>{
									let data =res.data;
									if (data.code == 200) {
										// console.log(this.globalData.Base64.decode( data.data.sentences));
										uni.showToast({
											title:"删除成功"
										})
										resolve(true);
									} else if (data.code == 520) {
										this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
										resolve(data.message);;
									} else {	
										uni.showToast({
											title:data.message,
											icon:'error'
										})
										resolve(false);
									}
								}).catch((e)=>{
									uni.showToast({
										title:"请求错误",
										icon:'error'
									})
									console.log(e);
									reject(e);
								})		
							}else{
								reject('取消');
							}
						}
					})
						
				})
			},
			//删除文件 end
			//上传文件 start
			audioUKFileSelect(e){
			  this.uploadfile(e.tempFilePaths[0],1).then(()=>{					
					this.audioWordUKFileExist=true;	
				});	
			},
			audioUSAFileSelect(e){
				this.uploadfile(e.tempFilePaths[0],2).then(()=>{					
					this.audioWordUSAFileExist=true;	
				});	;
			},
			audioUKFileDelete(e){
				this.deleteAudioFile(this.words.word.word,1).then((res)=>{
					if(res){
						this.audioWordUKFileExist=false;
					}
				});
			},
			audioUSAFileDelete(e){
				this.deleteAudioFile(this.words.word.word,2).then((res)=>{
					if(res){
						this.audioWordUSAFileExist=false;
					}
				});;
			},
			uploadfile(tempFile,type){
				// 使用 Promise then/catch 方式调用
				return new Promise((resolve,reject)=>{
					uni.uploadFile({
						url: this.globalData.url + 'adminFile/uploadAudio',
						filePath: tempFile,
						name: 'file',
						header: {
							token: uni.getStorageSync('token')
						},
						formData:{
							type,
							word:this.words.word.word.trim()
						}
					})
					.then(res => {
						// 此处的 res 参数，与使用默认方式调用时 success 回调中的 res 参数一致
						// console.log(res.data);
						
						let data = JSON.parse(res.data);
						if (data.code == 200) {
							uni.showToast({
								title: '上传成功'
							});
							resolve();
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
			//打开关闭有道翻译 start
			toggleYouDao(){
				this.showYoudao=!this.showYoudao;
				// #ifdef H5
				this.flexHeight=document.body.scrollHeight+"px";
				// #endif
				if(this.flexWidth==100){
					this.flexWidth=40;
				}else{
					this.flexWidth=100;
				}
			},
			//打开关闭有道翻译 end
			//添加例句 start
			addLiju(){
				this.words.liju.sentences.push({
					chinese: "",
					dict:"",
					english:"",
					
				});
				this.lijuCurrent=this.words.liju.sentences.length-1;
			},
			//添加例句 end
			//删除例句 start
			deleteLiju(index){
				uni.showModal({
					title:"删除例句",
					content:`确定要删除例句${index+1}吗？`,
					success: (res) => {
						if(res.confirm){
							this.words.liju.sentences.splice(index,1);
							this.lijuCurrent=this.words.liju.sentences.length-1;
						}
					}
				})
				
			},
			//删除例句 end
			//加载数据 start
			loadData(){
				if(this.mode=="update"){
					this.loadNetData().then(()=>{
						this.loadWordAudioFileExists(1).then((res)=>{	
							this.audioWordUKFileExist=res;		
							this.loadWordAudioFileExists(2).then((res)=>{
								this.audioWordUSAFileExist=res;	
							});
						});
					});
				}
			},
			//加载数据 end
			//加载录音文件是否存在 start
			loadWordAudioFileExists(type){
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminFile/getWordAudioFileExist",
							method:'POST',
							header:this.globalData.header.normal,
							dataType:"json",
							data:{
								word:this.words.word.word,
								type:type
							}
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								resolve(true);
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {								
								resolve(false);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})		
				})
			},
			//加载录音文件是否存在 end
			//加载服务器数据 start
			loadNetData(){
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminWord/getOneByInitialAndId",
							method:'POST',
							header:this.globalData.header.normal,
							dataType:"json",
							data:{
								initial:this.initial,
								id:this.id
							}
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.words = data.data;
								
			
								this.words.liju.sentences =this.globalData.Base64.decode(this.words.liju.sentences);  
								this.wordsBackUp = JSON.parse(JSON.stringify(this.words));
								// this.words.liju.sentences.forEach((item2,index2)=>{
								// 	item2.split = item2.english.split(" ");
								// 	let wordSplit = this.words.word.word.split(" ");
								// 	item2.styleIndexStart =this.globalData.matchingAtr(item2.split,wordSplit);
								// 	item2.styleIndexEnd =item2.styleIndexStart+ wordSplit.length;
									
								// })
							
								console.log(this.words);
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
