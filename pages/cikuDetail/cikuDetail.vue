<template>
	<view class="main">
		<my-navBack backColor="white"  background="linear-gradient(90deg, #0d0d0d, #595959)" title="词库详情"></my-navBack>

		<view class="header">
			<uni-row class="demo-uni-row ">
				<uni-col :span="11">
					<view style="padding: 40rpx;">
						<my-textHeadIcon
							v-if="cikuWordDatas.ciku.dscabb != ''"
							:text="cikuWordDatas.ciku.dscabb"
							:id="'cavasHead' + Math.round(Math.random() * 1000)"
							width="270"
							height="340"
						></my-textHeadIcon>
					</view>
				</uni-col>
				<uni-col :span="13">
					<view style="padding-top: 40rpx;">
						<view class="title">{{ cikuWordDatas.ciku.dsc }}</view>
						<view class="subTitle">{{ cikuWordDatas.ciku.dscabb }}</view>
						<view class="footerTitle">
							<view class="vabnum">
								<text>单词量: {{ cikuWordDatas.count }}</text>
							</view>
							
						</view>
					</view>
					
					<view style="margin-top: 20rpx;vertical-align: middle;">
						<view style="display: inline-block;vertical-align: middle;">
							<checkbox-group @change="allCheckChange">
								<text style="color: grey;margin-right: 20rpx;">全选:</text><checkbox value="allCheck" :checked="allCheck"></checkbox>
							</checkbox-group>
						</view>
						<button @click.stop="batchDelete" style="vertical-align: middle;margin-left: 20rpx;"  type="warn" size="mini"><uni-icons color="white" type="trash-filled" size="12"></uni-icons>批量删除
						</button>
						<button @click.stop="insertWordPop" style="vertical-align: middle;margin-left: 20rpx;"  type="primary" size="mini"><uni-icons color="white" type="plus" size="12"></uni-icons>添加单词
						</button>
					</view>
				</uni-col>
			</uni-row>
		</view>

		<!-- 		<view class="content" > -->

		<scroll-view :scroll-y="true" :style="{ height: `63vh` }" @scrolltolower="getDatas">
			<uni-collapse @change="change" accordion>
				<checkbox-group @change="checkChange">
				<template v-for="(item, index) in partWordFulls" :key="index" style="position: relative;">
		
					<uni-collapse-item  :showIndex="false" titleFontSize="30rpx"  >
						<template v-slot:title>
							<view style="padding: 50rpx;">
								<checkbox @click.stop="checkClick(index,checks[index])" :value="index+''" :checked="checks[index]">
									
								</checkbox>
								<view style="display: inline-block;margin-left: 30rpx;">
									<text style="margin-right: 30rpx;">
										{{index + 1 + '.'}}
									</text>
									<text>
										{{item.word.word}}
									</text>
								</view>
								<view style="display: inline-block;float: right;margin-right: 50rpx;">
									<view @click.stop="deleteSignle(index)" style="display: inline-block;vertical-align: middle;cursor: pointer;">
										<uni-icons color="red" type="trash-filled" size="20"></uni-icons>
									</view>
									
									<view @click.stop="updateWordPop(index)" style="display: inline-block;vertical-align: middle;margin-left: 20rpx;cursor: pointer;">
										<uni-icons color="blue" type="compose" size="20"></uni-icons>
									</view>
									
								</view>
							</view>
						</template>
						<uni-list>
							<uni-list-item
								v-if="item.inBook"
								:wid="item.word.id"
								:initial="item.word.word.slice(0, 1).toLowerCase()"
								:index="index"
								:showBook2="true"
								:word="item.word.word"
								:subTitle2="item.word.lijus"
								:titleUrl="`${globalData.mediaUrl}audio/1/${item.word.word.slice(0, 1)}/${item.word.word}.mp3`"
								:titleUrl2="`${globalData.mediaUrl}audio/2/${item.word.word.slice(0, 1)}/${item.word.word}.mp3`"
								:title2="`美式: /${item.word.soundmark2}/`"
								:title="`英式: /${item.word.soundmark1}/`"
								@titleTap="playUK"
								@title2Tap="playUSA"
								:subTitle="item.word.trans"
								titleFontColor="blue"
								titleBottomBorder="2px solid blue"
								subtitleSize="35rpx"
								backgroundColor="whitesmoke"
								@subTitle2Tap="playUKSentence"
								@subTitle2Tap2="playUSASentence"
								@subTitle2Tap3="playChinese"
								@deleteBookTap="deleteBookTap"
								@insertBookTap="insertBookTap"
								@getWord="getWord"
							></uni-list-item>
							<uni-list-item
								v-else="!item.inBook"
								:wid="item.word.id"
								:initial="item.word.word.slice(0, 1).toLowerCase()"
								:index="index"
								:showBook="true"
								:word="item.word.word"
								:subTitle2="item.word.lijus"
								:titleUrl="`${globalData.mediaUrl}audio/1/${item.word.word.slice(0, 1)}/${item.word.word}.mp3`"
								:titleUrl2="`${globalData.mediaUrl}audio/2/${item.word.word.slice(0, 1)}/${item.word.word}.mp3`"
								:title2="`美式: /${item.word.soundmark2}/`"
								:title="`英式: /${item.word.soundmark1}/`"
								@titleTap="playUK"
								@title2Tap="playUSA"
								:subTitle="item.word.trans"
								titleFontColor="blue"
								titleBottomBorder="2px solid blue"
								subtitleSize="35rpx"
								backgroundColor="whitesmoke"
								@subTitle2Tap="playUKSentence"
								@subTitle2Tap2="playUSASentence"
								@subTitle2Tap3="playChinese"
								@deleteBookTap="deleteBookTap"
								@insertBookTap="insertBookTap"
								@getWord="getWord"
							></uni-list-item>

							<!-- 			<uni-list-item 
												:subTitle2="item.word.lijus" 
												:titleUrl="`${globalData.mediaUrl}audio/1/${item.word.word.slice(0,1)}/${item.word.word}.mp3`" 
												:titleUrl2="`${globalData.mediaUrl}audio/2/${item.word.word.slice(0,1)}/${item.word.word}.mp3`"   
												:title2="`美式: /${item.word.soundmark2}/`"  
												:title="`英式: /${item.word.soundmark1}/`" 
												@titleTap="playUK" 
												@title2Tap="playUSA" 
												:subTitle="item.word.trans" 
												titleFontColor="blue" 
												titleBottomBorder="2px solid blue" 
												subtitleSize="35rpx"  
												backgroundColor="whitesmoke" 
												@subTitle2Tap="playUKSentence" 
												@subTitle2Tap2="playUSASentence">
												</uni-list-item> -->
						</uni-list>
					</uni-collapse-item>
					
				</template>
				</checkbox-group>
			</uni-collapse>
		</scroll-view>

		<!-- </view> -->
		
		<uni-popup ref="cikutypepopup"  type="center" @maskClick="cancelCikutypePop">
			<view style="background-color: black;padding: 20rpx;">
				<view style="justify-content: center;text-align: center;margin-bottom: 20rpx;">
					{{cikutypeModal=="insert"?'添加词库类别':'修改词库类别'}}
				</view>
				<scroll-view :scroll-y="true" style="height: 200rpx;">
					<view style="color: white;">
						<view v-if="wordEntity.length!=0" style="display: inline-block;">
							已选择单词：[<template v-for="(item,index) in wordEntity" :key="index">
								<view  @click="deleteChooseWord(index)" style="display: inline-block;padding: 20rpx;cursor: pointer;">
									{{item}},
								</view>
							</template>],共计{{wordEntity.length}}个单词
						</view>
						<view v-else>
							请选择单词
						</view>
					</view>
				</scroll-view>
		
				<view style="justify-content: center;text-align: center;">
					<button @click="showChooseWord" type="primary"   size="mini">单词选择器</button>
				</view>
				<view style="justify-content: center;text-align: center;">
					
					<button @click="insertCikuWord" v-if="cikutypeModal=='insert'" type="primary"  size="mini">添加</button>
					<button @click="updateCikuWord" v-else type="primary"   size="mini">修改</button>
					<button @click="cancelCikutypePop" style="margin-left: 20rpx;" type="warn" size="mini">取消</button>
				</view>
			
						
			</view>
		</uni-popup>
		<uni-popup ref="wordChoosepopup"  type="center" @maskClick="cancelChooseWordPop">
			<view style="justify-content: center;text-align: center;background-color:white;padding: 50rpx;">
				<view style="display: flex;flex-direction: row;flex-wrap: nowrap;padding: 20rpx;">
					<view style="flex: 5;font-size: 37rpx;font-style: oblique;">
						单词选择器
					</view>
					<view style="flex: 1;float: right;">
						<view @click.stop="cancelChooseWordPop" style="display: inline-block;vertical-align: middle;margin-left: 20rpx;cursor: pointer;">
							<uni-icons color="red" type="close" size="20"></uni-icons>
						</view>
					</view>
				</view>
				<scroll-view :scroll-x="true" style="width: 100vw;">
					<template v-for="(item,index) in wordTree" :key="index">
						<view @click="initialClick(this.wordTree[index].text)" style="display: inline-block;width: 5%;cursor: pointer;color: royalblue;">
							{{item.text}} 
						</view>
					</template>
				</scroll-view>
				<view style="padding: 20rpx;font-size: 33rpx;">
					<input type="text" placeholder-style="font-size:33rpx;padding:20rpx" @input="searchWordInput" v-model="searchWord" placeholder="搜索单词" style="border: 1rpx solid black;"/>
					精准查询：<switch @change="searchWordSwitchChange"></switch>
				</view>
				<scroll-view :scroll-y="true" style="height: 200rpx;">
					<view style="padding: 50rpx;border-bottom: 2rpx solid grey;">
						当前选择：{{chooseInitial}}
						<view v-if="wordEntity.length!=0" style="display: inline-block;">
							，[<template v-for="(item,index) in wordEntity" :key="index">
								<view   @click="deleteChooseWord(index)"  style="display: inline-block;padding: 20rpx;cursor: pointer;">
									{{item}},
								</view>
							</template>],共计{{wordEntity.length}}个单词
						</view>
					</view>
				</scroll-view>
		
				<scroll-view :scroll-y="true" style="height: 500rpx;">
					<template v-for="(item,index) in wordTreeChildren" :key="index">
						<view style="display: inline-block;position: relative;">
							<view @click="showWordAction(index)" style="pdisplay: inline-block;padding: 20rpx;;cursor: pointer;color: dimgrey;">
								{{item.word}} 
								
							</view>
							<view v-if="item.show" style="width: 50vw;;position: absolute;top: 70rpx;z-index: 90;;left: 0vw;display: inline-block;background-color: black;color: white;padding: 30rpx;">
								<view>
									{{item.word}} 
								</view>
								<view>
									{{item.trans}}
								</view>
								
							</view>
						</view>
					
					</template>
				</scroll-view>
				<view style="padding: 10rpx;">
					<button type="primary" @click="confirmChooseWord" >确定选择</button>
				</view>
				
			</view>
		</uni-popup>
	</view>
</template>

<script>
export default {
	data() {
		return {
			globalData: {},
			clockByInsertAndDelte: true,
			cikuWordDatas: {
				ciku: {
					createtime: '',
					deleted: 0,
					dsc: '',
					dscabb: '',
					id: 0,
					uid: 0,
					updatetime: ''
				}
			},
			isRecite: false,
			reciteText: '',
		
			cikuTypeId: 0,
			cikuId: 0,
			wordFulls: [],
			partWordFulls: [
				{
					inBook: false,
					word: {
						word: '',
						lijus: [
							{
								english: '',
								chinese: '',
								dict: ''
							}
						]
					}
				}
			],
			count: 1,
			baseNum: 11,
			isPlayingWord: false,
			random:1,
			
			allCheck:false,
			checks:[],
			checkData:[],
			cikutypeModal:"insert",
			cikutypeEntity:[],
			cikutypeEntityBackUp:[],
			
			wordEntity:[],
			cikuTypeRules:{
				dsc:{
					label:'词库类别名称',
					rules:[
						{
							require:true,
							errorMessage: '{label}不能为空，请填写',
						},
						{
							format: 'string',
							errorMessage: '{label}只为字符串'
						},
						{
							validateFunction:(rule, value, data, callback) => {
								// 异步需要返回 Promise 对象
								return new Promise((resolve, reject) => {
									let minLength = 1;
									let maxLength = 50;
									if (value.toString().length == 0) {
										reject(new Error('{label}不能为空，请填写'))
									} else if (value.toString().length > maxLength || value.toString().length < minLength) {
										reject(new Error('{label}长度应该在'+  minLength +"-" + maxLength + '位'));
									} else {
										uni.request({
												url:this.globalData.url+"adminCiku/getCikuTypeExist",
												method:'POST',
												dataType:'json',
												header:this.globalData.header.normal,
												data:{
													dsc:value
												},
												success: (res) => {
													let data =res.data;
													if(data.code==200){
														reject(new Error(data.message))
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
			},
			wordTree:[],
			wordTreeChildren:[],
			chooseInitial:'a',
			searchWord:'',
			searchType:3,
		};
	},

	onLoad(param) {
		
		this.cikuTypeId = param.cikutypeId;
		this.cikuId = param.cikuId;
		if(this.cikuId==undefined||this.cikuTypeId==undefined){
				uni.showToast({
					title:"参数错误",
					icon:'error'
				})
				setTimeout(()=>{
					uni.redirectTo({
						url:"/pages/index/index"
					});
				},500)
			
		}else{
			this.globalData = getApp().globalData;
			// param={
			// 	"cikuTypeId":2,
			// 	"cikuId":1
			// }
			
			if (this.globalData.checkLogin()) {
				this.loadData(param);
			}
		}
	
	},
	onShow(param) {
		this.globalData = getApp().globalData;	
		uni.setStorageSync("cikuTypeId",this.cikuTypeId);
		
	},
	methods: {
		//添加单词 start
		insertCikuWord(){
			console.log(this.cikutypeEntity);
			uni.request({
				url: this.globalData.url + 'adminCiku/insertBatch',
				dataType: 'json',
				method: 'POST',
				header: {
					'content-type': this.globalData.contentType.normalData,
					token: uni.getStorageSync('token')
				},
				data:{
					words:JSON.stringify(this.cikutypeEntity),
					cikuTypeId:this.cikuTypeId,
					cikuId:this.cikuId
				},
			}).then((res)=>{
				let data = res.data;
				if (data.code == 200) {
		
					uni.hideLoading();
					uni.showToast({
						title:"添加成功",
					})
					setTimeout(()=>{
						uni.redirectTo({
							url:`/pages/cikuDetail/cikuDetail?cikutypeId=${this.cikuTypeId}&cikuId=${this.cikuId}`
						})
					},500);
					// console.log(this.wordTreeChildren);
				} else if (data.code == 520) {
					uni.hideLoading();
					this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
				} else {
					uni.hideLoading();
					uni.showToast({
						title:"添加失败",
						icon:'error'
					})
					console.log(data.message);
				}
			}).catch((e)=>{
				uni.hideLoading();
				uni.showToast({
					title:"请求错误",
					icon:'error'
				})
				console.log(e);
			});
		},
		//添加单词 end
		//更新单词 start
		updateCikuWord(){
			if(this.cikutypeEntity.length>0){
				uni.request({
					url: this.globalData.url + 'adminCiku/update',
					dataType: 'json',
					method: 'POST',
					header: {
						'content-type': this.globalData.contentType.normalData,
						token: uni.getStorageSync('token')
					},
					data:{
						word:JSON.stringify(this.cikutypeEntity[0]),
						oldword:JSON.stringify(this.cikutypeEntityBackUp[0]),
						cikuTypeId:this.cikuTypeId,
						cikuId:this.cikuId
					},
				}).then((res)=>{
					let data = res.data;
					if (data.code == 200) {
						uni.hideLoading();
						uni.showToast({
							title:"修改成功",
						})
						setTimeout(()=>{
							uni.redirectTo({
								url:`/pages/cikuDetail/cikuDetail?cikutypeId=${this.cikuTypeId}&cikuId=${this.cikuId}`
							})
						},500);
						// console.log(this.wordTreeChildren);
					} else if (data.code == 520) {
						uni.hideLoading();
						this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
					} else {
						uni.hideLoading();
						uni.showToast({
							title:"修改失败",
							icon:'error'
						})
						
						console.log(data.message);
					}
				}).catch((e)=>{
					uni.hideLoading();
					uni.showToast({
						title:"请求错误",
						icon:'error'
					})
					console.log(e);
				});
			}else{
				uni.showToast({
					title:"未选中",
					icon:'error'
				})
			}
			
		},
		//更新单词 end
		//显示单词操作anctionSheet start
		showWordAction(index){
			let itemList=[];
			if(this.wordTreeChildren[index].show){
				itemList=[`选中/删除单词${this.wordTreeChildren[index].word}`,"隐藏单词意思"];
				
			}else{
				itemList=[`选中/删除单词${this.wordTreeChildren[index].word}`,"显示单词意思"];
			}
			uni.showActionSheet({
				itemList:itemList,
				success:  (res)=> {
						if(res.tapIndex==0){
							this.wordClick(index);
						}else{
							this.showWordTrans(index);
						}
						
					},
					fail: (res) =>{
						console.log(res.errMsg);
					}
			})
		},
		//显示单词操作anctionSheet end
		//显示单词译文 start
		showWordTrans(index){
			this.wordTreeChildren.forEach((item,indexx)=>{
				if(index!=indexx){
					item.show=false;
				}
				
				return item;
			})
			this.wordTreeChildren[index].show=!this.wordTreeChildren[index].show;	
		},
		//显示单词译文 end
		//开启精准查询 start
		searchWordSwitchChange(e){
			if(e.detail.value){
				//精准查询
				this.searchType=0;
			}else{
				this.searchType=3;
			}
			this.searchWordInput();
		},
		//开启精准查询 end
		//查询单词 start
		searchWordInput(e){
			if(e!=undefined){
				this.searchWord = e.detail.value.trim();
			}
			
			if(this.searchWord.length!=0){
				uni.request({
					url: this.globalData.url + 'adminWord/getOne',
					dataType: 'json',
					method: 'POST',
					header: {
						'content-type': this.globalData.contentType.normalData,
						token: uni.getStorageSync('token')
					},
					data:{
						word:this.searchWord,
						type:this.searchType,
					},
				}).then((res)=>{
					let data = res.data;
					if (data.code == 200) {
						
						
						this.wordTreeChildren =this.globalData.Base64.decode( data.data);
						this.wordTreeChildren=this.wordTreeChildren.concat();
						this.wordTreeChildren.forEach(item=>{
							item.show=false;
						})
						uni.hideLoading();
						// console.log(this.wordTreeChildren);
					} else if (data.code == 520) {
						uni.hideLoading();
						this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
					} else {
						uni.hideLoading();
						
						this.wordTreeChildren=[];
						console.log(data.message);
					}
				}).catch((e)=>{
					uni.hideLoading();
					uni.showToast({
						title:"请求错误",
						icon:'error'
					})
					console.log(e);
				});
			}else{
				this.initialClick(this.chooseInitial);
			}
			
		},
		//查询单词 end
		//删除选中的单词 start
		deleteChooseWord(index){
			uni.showModal({
				title:"删除选择单词",
				content:`您确定要删除单词${this.wordEntity[index]}吗？`,
				success: (res) => {
					if(res.confirm){
						this.wordEntity.splice(index,1);
						this.cikutypeEntity.splice(index,1);
					}
				}
			})
		},
		//删除选中的单词 end
		//单击单词选择器的单词 start
		wordClick(index){
		
			if(!this.wordEntity.includes(this.wordTreeChildren[index].word)){
				if(this.cikutypeModal=="insert"||this.wordEntity.length==0){
					this.wordEntity.push(this.wordTreeChildren[index].word);
					this.cikutypeEntity.push({
						initial:this.wordTreeChildren[index].word.slice(0,1),
						id:null,
						wid:this.wordTreeChildren[index].id,
						deleted:0,
						createtime:null,
						updatetime:null,
					});
				}else{
					uni.showToast({
						title:"修改单词不能多选",
						icon:'error'
					})
				}
				
			}else{
				let flag = false;
				let id = 0;
				for (let i = 0; i < this.wordEntity.length; i++) {
					if(this.wordEntity[i]==this.wordTreeChildren[index].word){
						flag=true;
						id = i;
						break;
					}
				}
				if(flag){
					this.wordEntity.splice(id,1);
				}
			}
		
			
			console.log(this.wordEntity);
		},
		//单击单词选择器的单词 end
		//单击单词选择器的initial start
		initialClick(initial){
			this.chooseInitial=initial;
			uni.showLoading({
				title:"加载中"
			})
			uni.request({
				url: this.globalData.url + 'adminWord/getInitial',
				dataType: 'json',
				method: 'POST',
				header: {
					'content-type': this.globalData.contentType.normalData,
					token: uni.getStorageSync('token')
				},
				data:{
					initial:initial
				},
			}).then((res)=>{
				let data = res.data;
				if (data.code == 200) {
					
					
					this.wordTreeChildren =this.globalData.Base64.decode( data.data);
					this.wordTreeChildren=this.wordTreeChildren.concat();
					this.wordTreeChildren.forEach(item=>{
						item.show=false;
					})
					uni.hideLoading();
					console.log(this.wordTreeChildren);
				} else if (data.code == 520) {
					uni.hideLoading();
					this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
				} else {
					uni.hideLoading();
					uni.showToast({
						title:data.message,
						icon:'error'
					})
					console.log(data.message);
				}
			}).catch((e)=>{
				uni.hideLoading();
				uni.showToast({
					title:"请求错误",
					icon:'error'
				})
				console.log(e);
			});
		},
		//关闭单词选择器 start
		cancelChooseWordPop(){
			 this.$refs.wordChoosepopup.close();
			 this.cikutypeEntity=[];
			 this.wordEntity=[];
		},
		//关闭单词选择器 end
		//确定单词选择器 start
		confirmChooseWord(){
			this.$refs.wordChoosepopup.close();
		},
		//确定单词选择器 end
		//显示单词选择器 start
		showChooseWord(){
			this.loadWordTree();
			 this.$refs.wordChoosepopup.open();
			
		},
		//显示单词选择器 end
		//加载wordtree start
		loadWordTree(){
			uni.request({
				url: this.globalData.url + 'adminWord/choosePick',
				dataType: 'json',
				method: 'POST',
				header: {
					'content-type': this.globalData.contentType.normalData,
					token: uni.getStorageSync('token')
				},
			}).then((res)=>{
				let data = res.data;
				if (data.code == 200) {
					uni.showToast({
						title:data.message
					})
					
					this.wordTree=data.data;
					this.wordTree=this.wordTree.concat();
					this.initialClick('a');
				} else if (data.code == 520) {
					this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
				} else {
					uni.showToast({
						title:data.message,
						icon:'error'
					})
					console.log(data.message);
				}
			}).catch((e)=>{
				uni.showToast({
					title:"请求错误",
					icon:'error'
				})
				console.log(e);
			});
			
		},
		//加载wordtree end
		//添加单词 start
		insertWordPop(){
		
			this.cikutypeModal="insert";
			 this.$refs.cikutypepopup.open();
		},
		//添加单词 end
		//更新单词 start
		updateWordPop(index){
		
			this.cikutypeModal="update";
			this.wordEntity=[this.partWordFulls[index].word.word];
			this.cikutypeEntity=[];
			this.cikutypeEntity.push({initial:this.partWordFulls[index].word.word.slice(0,1),wid:this.partWordFulls[index].word.id})
			this.cikutypeEntityBackUp=[];
			this.cikutypeEntityBackUp.push(JSON.parse(JSON.stringify({initial:this.partWordFulls[index].word.word.slice(0,1),wid:this.partWordFulls[index].word.id})));
			this.$refs.cikutypepopup.open();
		},
		//更新单词 end
		//取消弹出词库类别框 start
		cancelCikutypePop(){
			this.cikutypeEntity=JSON.parse(JSON.stringify(this.cikutypeEntityBackUp));
			this.wordEntity=[];
			 this.$refs.cikutypepopup.close();
		},
		//取消弹出词库类别框 end
		//删除单个 start
		deleteSignle(index){
			uni.showModal({
				title:"删除",
				content:`您确定要删除【${this.partWordFulls[index].word.word}】吗？`,
				success: (res) => {
					if(res.confirm){
						uni.request({
							url: this.globalData.url + 'adminCiku/deleteSingle',
							dataType: 'json',
							method: 'POST',
							header: {
								'content-type': this.globalData.contentType.normalData,
								token: uni.getStorageSync('token')
							},
							data: {
								word:JSON.stringify({initial:this.partWordFulls[index].word.word.slice(0,1),wid:this.partWordFulls[index].word.id}),
								cikuTypeId:this.cikuTypeId,
								cikuId:this.cikuId
							},
							success: res => {
								let data = res.data;
								if (data.code == 200) {
									uni.showToast({
										title:data.message
									})
									setTimeout(()=>{
										uni.redirectTo({
											url:`/pages/cikuDetail/cikuDetail?cikutypeId=${this.cikuTypeId}&cikuId=${this.cikuId}`
										})
									},500)
								
								} else if (data.code == 520) {
									this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								} else {
									uni.showToast({
										title:data.message,
										icon:'error'
									})
									console.log(data.message);
								}
						
							
							},
							fail: e => {
								uni.showToast({
									title:"请求错误",
									icon:'error'
								})
								console.log(e);
							
							}
						});
					}
				}
			});
		},
		//删除单个 end
		//批量删除 start
		batchDelete(){
			if(this.checkData.length==0){
				uni.showToast({
					title:"未选中！",
					icon:'error'
				})
				return;
			}
			uni.showModal({
				title:"批量删除",
				content:"您确定要删除选中的单词吗？",
				success: (res) => {
					if(res.confirm){
						uni.request({
							url: this.globalData.url + 'adminCiku/deleteBatch',
							dataType: 'json',
							method: 'POST',
							header: {
								'content-type': this.globalData.contentType.normalData,
								token: uni.getStorageSync('token')
							},
							data: {
								words:JSON.stringify(this.checkData),
								cikuTypeId:this.cikuTypeId,
								cikuId:this.cikuId
							},
							success: res => {
								let data = res.data;
								if (data.code == 200) {
									uni.showToast({
										title:data.message
									})
									setTimeout(()=>{
										uni.redirectTo({
											url:`/pages/cikuDetail/cikuDetail?cikutypeId=${this.cikuTypeId}&cikuId=${this.cikuId}`
										})
									},500)
								
								} else if (data.code == 520) {
									this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								} else {
									uni.showToast({
										title:data.message,
										icon:'error'
									})
									console.log(data.message);
								}
						
							
							},
							fail: e => {
								uni.showToast({
									title:"请求错误",
									icon:'error'
								})
								console.log(e);
							
							}
						});
					}
				}
			});
		},
		//批量删除 end
		//单选click start
		checkClick(index){
			console.log(this.checks[index]);
			let initial = this.partWordFulls[index].word.word.slice(0,1);
			let wid = this.partWordFulls[index].word.id;
			if(this.checks[index]){
				//选中
				let flag =false;
				let index2 = 0;
				if(this.checkData.length>0){
					
					for (var i = 0; i < this.checkData.length; i++) {
						if(this.checkData[i].initial==initial&&this.checkData[i].wid==wid){
							flag=true;
							index2=i;
							break;
						}
					}
					
					if(flag){
						this.checkData.splice(index2,1);
					}
					
				}
				
			}else{
				//取消选中
				let flag =true;
				for (var i = 0; i < this.checkData.length; i++) {
					if(this.checkData[i].initial==initial&&this.checkData[i].wid==wid){
						flag=false;
						break;
					}
				}
				if(flag){
					this.checkData.push({initial,wid})
				}
			}
			this.checks[index]=!this.checks[index];
			console.log(this.checkData);
			
		},
		//单选click end
		//单选change start
		checkChange(e){
			
		},
		//单选change end
		//全选change start
		allCheckChange(e){
			this.checkData=[];
			if(e.detail.value.length!=0){
				//全选
				for (let i = 0; i < this.wordFulls.length; i++) {
					this.checks[i]=true;
					this.checkData.push({initial:this.wordFulls[i].word.word.slice(0, 1),wid:this.wordFulls[i].word.id});
				}
			}else{
				//取消全选
				for (let i = 0; i < this.wordFulls.length; i++) {
					this.checks[i]=false;
				}
			}
			console.log(this.checkData);
		},
		//全选change end
		//添加到生词库 start
		insertBookTap(e) {
			console.log(e);
			if (this.clockByInsertAndDelte) {
				uni.showLoading({
					title: '加载数据中'
				});
				this.clockByInsertAndDelte = false;
				uni.request({
					url: this.globalData.url + 'freshword/insertOneByUid',
					method: 'post',
					dataType: 'json',
					header: {
						'content-type': this.globalData.contentType.normalData,
						token: uni.getStorageSync('token')
					},
					data: {
						uid: JSON.parse(uni.getStorageSync('user')).id,
						initial: e.initial,
						wid: parseInt(e.id)
					},
					success: res => {
						let data = res.data;
						if (data.code == 200) {
							if (e.type == 'main') {
								this.partWordFulls[e.index].inBook = true;
							} else {
								this.partWordFulls[e.index].word.lijus[e.indexx].englishWord[e.indexy].fullwords[e.indexz].inBook = true;
							}
						} else if (data.code == 520) {
							this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
						} else {
							console.log(data.message);
						}
						this.clockByInsertAndDelte = true;
						uni.hideLoading();
					},
					fail: e => {
						console.log(e);
						this.clockByInsertAndDelte = true;
						uni.hideLoading();
					}
				});
			}
		},
		//添加到生词库 end
		//删除到生词库 start
		deleteBookTap(e) {
			console.log(e);
			if (this.clockByInsertAndDelte) {
				uni.showLoading({
					title: '加载数据中'
				});
				this.clockByInsertAndDelte = false;
				uni.request({
					url: this.globalData.url + 'freshword/deleteOneByUid',
					method: 'post',
					dataType: 'json',
					header: {
						'content-type': this.globalData.contentType.normalData,
						token: uni.getStorageSync('token')
					},
					data: {
						uid: JSON.parse(uni.getStorageSync('user')).id,
						initial: e.initial,
						wid: parseInt(e.id)
					},
					success: res => {
						let data = res.data;
						if (data.code == 200) {
							if (e.type == 'main') {
								this.partWordFulls[e.index].inBook = false;
							} else {
								this.partWordFulls[e.index].word.lijus[e.indexx].englishWord[e.indexy].fullwords[e.indexz].inBook = false;
							}
						} else if (data.code == 520) {
							this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
						} else {
							console.log(data.message);
						}
						this.clockByInsertAndDelte = true;
						uni.hideLoading();
					},
					fail: e => {
						console.log(e);
						this.clockByInsertAndDelte = true;
						uni.hideLoading();
					}
				});
			}
		},
		//删除到生词库 end
		//点击例句中的单词 start
		getWord(e) {
			console.log('parent', e);
			if (!e.word.show) {
				this.partWordFulls[e.index].word.lijus.forEach(item => {
					item.englishWord.forEach(item2 => {
						item2.show = false;
					});
				});
			}
			// this.partWordFulls.wordFulls[e.index].words[e.index2].word.lijus[e.index3].englishWord[e.index4].show=!this.partWordFulls.wordFulls[e.index].words[e.index2].word.lijus[e.index3].englishWord[e.index4].show;
			this.partWordFulls[e.index].word.lijus[e.index3].englishWord[e.index4].show = !this.partWordFulls[e.index].word.lijus[e.index3].englishWord[e.index4].show;
			if (e.word.show) {
				uni.showLoading({
					title: '加载数据中'
				});
				uni.request({
					url: this.globalData.url + 'word/getOne',
					dataType: 'json',
					method: 'POST',
					header: {
						'content-type': this.globalData.contentType.normalData,
						token: uni.getStorageSync('token')
					},
					data: {
						word: e.word.word,
						uid: JSON.parse(uni.getStorageSync('user')).id,
						type: 1
					},
					success: res => {
						let data = res.data;
						if (data.code == 200) {
							this.partWordFulls[e.index].word.lijus[e.index3].englishWord[e.index4].fullwords = data.data;
							console.log(this.partWordFulls[e.index].word.lijus[e.index3].englishWord[e.index4]);
						} else if (data.code == 520) {
							this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
						} else {
							this.partWordFulls[e.index].word.lijus[e.index3].englishWord[e.index4].fullwords = undefined;
							console.log('erro', this.partWordFulls[e.index].word.lijus[e.index3].englishWord[e.index4]);
							console.log(data.message);
						}

						uni.hideLoading();
					},
					fail: e => {
						console.log(e);
						uni.hideLoading();
					}
				});
			}
		},
		//点击例句中的单词 end
		//展开折叠框 start
		change(e) {
			console.log(e, typeof e, e == '');
			if (e != '') {
				console.log(12);
				uni.showLoading({
					title: '加载数据中'
				});
				let indexs = parseInt(e);
				uni.request({
					url: this.globalData.url + 'liju/getByWord',
					dataType: 'json',
					method: 'POST',
					header: this.globalData.contentType.normal,
					data: {
						word: this.partWordFulls[indexs].word.word,
						limit:-1
					},
					success: res => {
						let data = res.data;
						if (data.code == 200) {
							// console.log(this.globalData.Base64.decode( data.data.sentences));
							let lijus = this.globalData.Base64.decode(data.data.sentences);
							this.partWordFulls[indexs].word.lijus=[];
							this.partWordFulls[indexs].word.lijus = lijus;
							this.partWordFulls.forEach((item, index) => {
								if (indexs == index) {
									item.word.lijus.forEach((item3, index3) => {
										let splits = item3.english.split(' ');
										this.partWordFulls[index].word.lijus[index3].englishWord = [];
										item3.englishWord = [];
										let wordSplit = item.word.word.split(" ");
										let styleIndexStart =this.globalData.matchingAtr(splits,wordSplit);
										let styleIndexEnd =styleIndexStart+ wordSplit.length;
										splits.forEach((item4, index4) => {
											let obj = {
												show: false,
												word: item4,
												styleIndexStart,
												styleIndexEnd
											};
											item3.englishWord.push(obj);
											return item4;
										});
										return item3;
									});
								}
								
								return item;
							});
							this.partWordFulls=this.partWordFulls.concat();
							console.log(this.partWordFulls);
						} else {
							console.log(data.message);
						}
						uni.hideLoading();
					},
					fail: e => {
						console.log(e);
						uni.hideLoading();
					}
				});
			}
		},
		//展开折叠框 end
		//下滑刷新 start
		getDatas(e) {
			console.log(e);
			uni.showLoading({
				title: '加载中'
			});
			this.count++;
			this.partWordFulls = this.partWordFulls.concat(this.wordFulls.slice((this.count - 1) * this.baseNum, this.count * this.baseNum));
			uni.hideLoading();
		},
		//下滑刷新 end
		//播放中文 start
		playChinese(e){
			console.log(e);
			if (e != undefined && e != 'undefined') {
				
				let url = this.globalData.audioSentenceUrl + 'type=2&' + 'audio=' + encodeURIComponent(e)+'&le=zh';
				this.playWordSound(url);
			}
		},
		//播放中文 end
		//播放英音句子 start
		playUKSentence(e) {
			if (e != undefined && e != 'undefined') {
				let url = this.globalData.audioSentenceUrl + 'type=1&' + 'audio=' + e;
				console.log(url);
				this.playWordSound(url);
			}
		},
		//播放英音句子 end
		//播放美音句子 start
		playUSASentence(e) {
			if (e != undefined && e != 'undefined') {
				let url = this.globalData.audioSentenceUrl + 'type=2&' + 'audio=' + e;
				console.log(url);
				this.playWordSound(url);
			}
		},
		//播放美音句子 end
		//播放英音 start
		playUK(e) {
			this.playWordSound(e);
		},
		//播放英音 end
		//播放美音 start
		playUSA(e) {
			this.playWordSound(e);
		},
		//播放美音 end
		//播放读音  start
		playWordSound(url) {
			console.log(url);
			if (!this.isPlayingWord) {
				const innerAudioContext = uni.createInnerAudioContext();
				innerAudioContext.autoplay = true;
				innerAudioContext.src = url;
				innerAudioContext.onPlay(() => {
					this.isPlayingWord = true;
					console.log('开始播放');
				});
				innerAudioContext.onEnded(() => {
					this.isPlayingWord = false;
				});
				innerAudioContext.onError(res => {
					console.log(res.errMsg);
					console.log(res.errCode);
				});
			}
		},
		//播放读音  end
		//加载数据 start
		loadData() {
			this.loadCikuWordData(this.cikuTypeId, this.cikuId);
		},
		//加载数据 end
		//获取服务器数据 start
		loadCikuWordData(cikuTypeId, cikuId) {
			uni.showLoading({
				title: '加载数据'
			});
			uni.request({
				url: this.globalData.url + 'wordFull/get',
				method: 'POST',
				dataType: 'json',
				header: {
					'content-type': this.globalData.contentType.normalData,
					token: uni.getStorageSync('token')
				},
				data: {
					cikuTypeId: parseInt(cikuTypeId),
					cikuId: parseInt(cikuId),
					uid: JSON.parse(uni.getStorageSync('user')).id
				},
				success: res => {
					uni.hideLoading();
					let data = res.data;
					if (data.code == 200) {
						uni.showToast({
							title: '获取成功'
						});
						this.cikuWordDatas = this.globalData.Base64.decode(data.data);

						this.wordFulls = [];
						this.count = 1;
						this.partWordFulls = [];
						this.checks=[];
						console.log('all',this.cikuWordDatas);
						this.cikuWordDatas.wordFulls.forEach(item => {
							if (item.words != undefined) {
								item.words.forEach(item2 => {
									this.checks.push(false);
									this.wordFulls.push(item2);
								});
							}
						});
						this.partWordFulls = this.wordFulls.slice((this.count - 1) * this.baseNum, this.count * this.baseNum);
						this.partWordFulls=this.partWordFulls.concat();
						console.log('finaly',this.partWordFulls);
						// console.log(this.cikuWordDatas);
					} else if (data.code == 520) {
						this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
					} else {
						uni.showToast({
							title: '获取失败',
							icon: 'error'
						});
						uni.navigateBack();
					}
				},
				fail: error => {
					uni.hideLoading();
					uni.showToast({
						title: '获取失败2',
						icon: 'error'
					});
					uni.navigateBack();
				}
			});
		},
		//获取服务器数据 end
		
	}
};
</script>

<style>
.text2 {
	word-wrap: break-word;
	word-break: break-all;
	overflow: hidden;
	text-overflow: ellipsis;
	display: -webkit-box;
	-webkit-box-orient: vertical;
	-webkit-line-clamp: 4; /*超出2行就显示省略号，可以填3或者其它正整数 */
}
.header {
	border-bottom: 3px dashed grey;
}
.content {
	height: 100%;
}
.title {
	font-size: 40rpx;
	line-height: 60rpx;
	font-weight: bolder;
}
.subTitle {
	/* text-indent: 40rpx; */
	font-size: 30rpx;
	line-height: 40rpx;
	margin-top: 10rpx;
	/* background-color: aqua; */
	height: 165rpx;
}
.footerTitle {
}
.vabnum {
	color: grey;
	display: inline-block;
	font-size: 32rpx;
}
.fav {
	margin-left: 85rpx;
	display: inline-block;
}
.wordBody {
	padding: 20rpx;
	font-size: 30rpx;
	background-color: whitesmoke;
}
.text1 {
}
.text2 {
	margin-top: 40rpx;
}
.sm1 {
}
.sm2 {
	margin-left: 10rpx;
}
</style>
