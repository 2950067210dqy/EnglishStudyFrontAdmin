<template>
	<!-- #ifdef APP-NVUE -->
	<cell>
		<!-- #endif -->

		<view
			:class="{ 'uni-list-item--disabled': disabled }"
			:hover-class="(!clickable && !link) || disabled || showSwitch ? '' : 'uni-list-item--hover'"
			:style="{'backgroundColor':backgroundColor}" 
			class="uni-list-item"
		
		>
			<view v-if="!isFirstChild" @tap="onClick" class="border--left" :class="{ 'uni-list--border': border }"></view>
			<view class="uni-list-item__container" :class="{ 'container--right': showArrow || link, 'flex--direction': direction === 'column' }">
				<slot name="header" >
					<view class="uni-list-item__header" 	@tap="onClick">
						<view v-if="thumb" class="uni-list-item__icon"><image :src="thumb" class="uni-list-item__icon-img" :class="['uni-list--' + thumbSize]" /></view>
						<view v-else-if="showExtraIcon" class="uni-list-item__icon"><uni-icons :color="extraIcon.color" :size="extraIcon.size" :type="extraIcon.type" /></view>
					</view>
				</slot>
				<slot name="body">
					<view class="uni-list-item__content" @tap="onClick" :class="{ 'uni-list-item__content--center': thumb || showExtraIcon || showBadge || showSwitch }">
						

						<view v-if="title"  class="uni-list-item__content-title" :class="[ellipsis !== 0 && ellipsis <= 2 ? 'uni-ellipsis-' + ellipsis : '']" >
							<text :style="{'color':titleFontColor,'borderBottom':titleBottomBorder,'display':'inline-block'}" @tap.stop="titleTap" :data-url="titleUrl">{{ title }}</text>
							&nbsp;&nbsp;&nbsp;&nbsp;
							<text :style="{'color':titleFontColor,'borderBottom':titleBottomBorder,'display':'inline-block'}" :data-url="titleUrl2" @tap.stop="title2Tap">{{ title2 }}</text>
						</view>
						<text v-if="subTitle" :style="{'fontSize':subtitleSize,'marginTop':'30rpx'}" class="uni-list-item__content-subTitle">{{ subTitle }}</text>
						<view v-if="showBookToggle">
							<button v-if="showBook" data-type="main" :data-index="index" :data-index2="index2" :data-id="wid" :data-initial="initial" style="width: 100%;" type="primary" @click.stop="insertBook">添加到生词本</button>
							<button v-if="showBook2" data-type="main" :data-index="index" :data-index2="index2" :data-id="wid" :data-initial="initial" style="width: 100%;" type="warn" @click.stop="deleteBook">从生词本中删除</button>
						</view>
						
					
						<text v-if="subTitle2.length!=0" style="font-size: 40rpx;color: black;margin-top: 30rpx;" class="uni-list-item__content-subTitle">例句:</text>
						<scroll-view  v-if="subTitle2.length!=0" scroll-y="true" style="height: 550rpx;border-bottom: 2px solid grey;">
							
							<template v-for="(item, indexx) in subTitle2"  :key="indexx">
								
								<view style="padding-bottom: 50rpx;">
									<view   :style="{'fontSize':subtitleSize,'marginTop':'30rpx','color':'black','height':'auto'}" class="uni-list-item__content-subTitle">
										<text style="margin-right: 20rpx;">{{indexx+1}}.</text>
										
										<template v-for="(item2, indexy) in item.englishWord==undefined?item.splitDatas:item.englishWord"  :key="indexy">
											
											<view style="display: inline-block;position: relative;" @click.stop="getWord(item2,index,index2,indexx,indexy)">
												<!-- <text style="font-weight: bolder;color: goldenrod;" v-if="item2.word.toLowerCase().includes(word.toLowerCase())">{{item2.word}}&nbsp;&nbsp;</text><text v-else>{{item2.word}}&nbsp;&nbsp;</text> -->	
												<text style="font-weight: bolder;color: goldenrod;" v-if="indexy>=item2.styleIndexStart&&indexy<item2.styleIndexEnd">{{item2.word}}&nbsp;&nbsp;</text><text v-else>{{item2.word}}&nbsp;&nbsp;</text>	
												<view v-if="item2.show"  style=" position: absolute;top: 30;left: 0;z-index: 3;background-color: black;color: white;padding: 20rpx;">
															<view v-if="item2.fullwords!=undefined">
																<!-- {{item2.fullword}} -->
																<scroll-view :scroll-y="true" style="height: 350rpx;">
																	<template v-for="(item3, indexz) in item2.fullwords"  :key="indexz">
																		<view style="padding: 5rpx;border-bottom: 5px dashed white;" >
																			<view style="font-size: 42rpx;font-weight: bolder;display: inline-block;">
																				{{item3.word.word}}
																				<view v-if="showBookToggle">
																					<button data-type="sub" :data-id="item3.word.id" :data-initial="item3.word.word.slice(0,1).toLowerCase()" :data-index="index" :data-index2="index2" :data-indexx="indexx" :data-indexy="indexy" :data-indexz="indexz"  @click.stop="deleteBook" v-if="item3.inBook"  style="font-size: 20rpx;" type="warn">从生词本中删除</button>
																					<button data-type="sub" v-else :data-id="item3.word.id" :data-initial="item3.word.word.slice(0,1).toLowerCase()" :data-indexx="indexx" :data-indexy="indexy" :data-indexz="indexz" :data-index="index" :data-index2="index2" @click.stop="insertBook" style="font-size: 20rpx;" type="primary">添加到生词本</button>
																				</view>
																				
																			</view>
																			<view style="font-size: 36rpx;font-weight: bolder;">
																				【音标】
																			</view>
																			<view   class="uni-list-item__content-title" :class="[ellipsis !== 0 && ellipsis <= 2 ? 'uni-ellipsis-' + ellipsis : '']" >
																				<text :style="{'color':'whitesmoke','borderBottom':'whitesmoke','display':'inline-block'}" @tap.stop="titleTap" :data-url="`${globalData.mediaUrl}audio/1/${item3.word.word.slice(0,1).toLowerCase()}/${item3.word.word}.mp3`">{{ '英:/'+item3.word.soundmark1+'/' }}</text>
																				&nbsp;&nbsp;&nbsp;&nbsp;
																				<text :style="{'color':'whitesmoke','borderBottom':'whitesmoke','display':'inline-block'}" :data-url="`${globalData.mediaUrl}audio/2/${item3.word.word.slice(0,1).toLowerCase()}/${item3.word.word}.mp3`" @tap.stop="title2Tap">{{ '美:/'+item3.word.soundmark2+'/' }}</text>
																			</view>
																			<view style="font-size: 36rpx;font-weight: bolder;">
																				【词意】
																			</view>
																			<view style="color: floralwhite;font-size: 33rpx;">
																				{{item3.word.trans}}
																			</view>
																		</view>
																	</template>
															
																</scroll-view>
																
																
															</view>
															<view v-else>
																{{item2.word}}
															</view>
												</view>
											</view>
										</template>
										<!-- {{item.english}} -->
										<view style="display: inline-block;font-size: 20rpx;color: grey;border-bottom: 1rpx solid grey;" @tap.stop="subTitle2Tap" :data-sentence1="item.english">
											<uni-icons type="headphones" color="grey" size="20"></uni-icons>
												UK
										</view> 
										<view  style="display: inline-block;font-size: 20rpx;color: grey;margin-left: 20rpx;border-bottom: 1rpx solid grey;" @tap.stop="subTitle2Tap2" :data-sentence2="item.english">
											<uni-icons type="headphones" color="grey" size="20"></uni-icons>
												USA
										</view> 
									</view>
									<text :style="{'fontSize':subtitleSize,'marginTop':'30rpx','color':'black','display':'inline-block'}" class="uni-list-item__content-subTitle">
										<text style="margin-right: 30rpx;"></text>{{item.chinese}}
										<view  style="display: inline-block;font-size: 20rpx;color: grey;margin-left: 20rpx;border-bottom: 1rpx solid grey;" @tap.stop="subTitle2Tap3" :data-sentence3="item.chinese">
											<uni-icons type="headphones" color="grey" size="18"></uni-icons>
										</view>
									</text>
									<text :style="{'fontSize':subtitleSize,'marginTop':'30rpx','color':'grey','display':'inline-block'}" class="uni-list-item__content-subTitle">
										<text style="margin-right: 30rpx;"></text>{{item.dict}}
									</text>
								</view>	
							</template>
						</scroll-view>
						
						
						<text v-if="note" class="uni-list-item__content-note">
							{{ note }}
							<text v-if="note2" class="uni-list-item__content-note2 ">{{ note2 }}</text>
						</text>
					</view>
				</slot>
				<slot name="footer">
					<view v-if="rightText || showBadge || showSwitch" class="uni-list-item__extra" :class="{ 'flex--justify': direction === 'column' }">
						<text v-if="rightText" class="uni-list-item__extra-text">{{ rightText }}</text>
						<uni-badge v-if="showBadge" :type="badgeType" :text="badgeText" :custom-style="badgeStyle" />
						<switch v-if="showSwitch" :disabled="disabled" :checked="switchChecked" @change="onSwitchChange" />
					</view>
				</slot>
			</view>
			<uni-icons v-if="showArrow || link" :size="16" class="uni-icon-wrapper" color="#bbb" type="arrowright" />
		</view>
		<!-- #ifdef APP-NVUE -->
	</cell>
	<!-- #endif -->
</template>

<script>
/**
 * ListItem 列表子组件
 * @description 列表子组件
 * @tutorial https://ext.dcloud.net.cn/plugin?id=24
 * @property {String} 	title 							标题
 * @property {String} 	note 							描述
 * @property {String} 	thumb 							左侧缩略图，若thumb有值，则不会显示扩展图标
 * @property {String}  	thumbSize = [lg|base|sm]		略缩图大小
 * 	@value 	 lg			大图
 * 	@value 	 base		一般
 * 	@value 	 sm			小图
 * @property {String} 	badgeText						数字角标内容
 * @property {String} 	badgeType 						数字角标类型，参考[uni-icons](https://ext.dcloud.net.cn/plugin?id=21)
 * @property {Object}   badgeStyle           数字角标样式
 * @property {String} 	rightText 						右侧文字内容
 * @property {Boolean} 	disabled = [true|false]			是否禁用
 * @property {Boolean} 	clickable = [true|false] 		是否开启点击反馈
 * @property {String} 	link = [navigateTo|redirectTo|reLaunch|switchTab] 是否展示右侧箭头并开启点击反馈
 *  @value 	navigateTo 	同 uni.navigateTo()
 * 	@value redirectTo 	同 uni.redirectTo()
 * 	@value reLaunch   	同 uni.reLaunch()
 * 	@value switchTab  	同 uni.switchTab()
 * @property {String | PageURIString} 	to  			跳转目标页面
 * @property {Boolean} 	showBadge = [true|false] 		是否显示数字角标
 * @property {Boolean} 	showSwitch = [true|false] 		是否显示Switch
 * @property {Boolean} 	switchChecked = [true|false] 	Switch是否被选中
 * @property {Boolean} 	showExtraIcon = [true|false] 	左侧是否显示扩展图标
 * @property {Object} 	extraIcon 						扩展图标参数，格式为 {color: '#4cd964',size: '22',type: 'spinner'}
 * @property {String} 	direction = [row|column]		排版方向
 * @value row 			水平排列
 * @value column 		垂直排列
 * @event {Function} 	click 							点击 uniListItem 触发事件
 * @event {Function} 	switchChange 					点击切换 Switch 时触发
 */
export default {
	name: 'UniListItem',
	emits: ['click', 'switchChange', 'titleTap', 'title2Tap','subTitle2Tap','subTitle2Tap2','subTitle2Tap3','insertBookTap','deleteBookTap','getWord'],
	props: {
		backgroundColor:{
			type: String,
			default: 'inherit'
		},
		direction: {
			type: String,
			default: 'row'
		},
		word: {
			type: String,
			default: ''
		},
		wid: {
			type: Number,
			default: 0
		},
		index: {
			type: Number,
			default: 0
		},
		index2: {
			type: Number,
			default:-1
		},
		initial: {
			type: String,
			default: ''
		},
		showWordDetail:{
			type:Array,
			default:[]
		},
		title: {
			type: String,
			default: ''
		},
		title2: {
			type: String,
			default: ''
		},
		titleFontColor:{
			type: String,
			default: 'black'
		},
		titleBottomBorder:{
			type: String,
			default: 'none'
		},
		titleUrl: {
			type: String,
			default: ''
		},
		titleUrl2: {
			type: String,
			default: ''
		},
		subTitle: {
			type: String,
			default: ''
		},
		subTitle2:{
			type:Array,
			default:[]
		},
		subTitle2Url :{
			type: String,
			default: ''
		},
		subtitleSize:{
			type: String,
			default: '30rpx'
		},
		showBookToggle:{
			type:Boolean,
			default:false
		},
		showBook:{
			type:Boolean,
			default:false
		},
		showBook2:{
			type:Boolean,
			default:false
		},
		note: {
			type: String,
			default: ''
		},
		note2: {
			type: String,
			default: ''
		},
		ellipsis: {
			type: [Number, String],
			default: 0
		},
		disabled: {
			type: [Boolean, String],
			default: false
		},
		clickable: {
			type: Boolean,
			default: false
		},
		showArrow: {
			type: [Boolean, String],
			default: false
		},
		link: {
			type: [Boolean, String],
			default: false
		},
		to: {
			type: String,
			default: ''
		},
		showBadge: {
			type: [Boolean, String],
			default: false
		},
		showSwitch: {
			type: [Boolean, String],
			default: false
		},
		switchChecked: {
			type: [Boolean, String],
			default: false
		},
		badgeText: {
			type: String,
			default: ''
		},
		badgeType: {
			type: String,
			default: 'success'
		},
		badgeStyle: {
			type: Object,
			default() {
				return {};
			}
		},
		rightText: {
			type: String,
			default: ''
		},
		thumb: {
			type: String,
			default: ''
		},
		thumbSize: {
			type: String,
			default: 'base'
		},
		showExtraIcon: {
			type: [Boolean, String],
			default: false
		},
		extraIcon: {
			type: Object,
			default() {
				return {
					type: '',
					color: '#000000',
					size: 20
				};
			}
		},
		border: {
			type: Boolean,
			default: true
		}
	},
	// inject: ['list'],
	data() {
		return {
			isFirstChild: false,
			globalData:{},
		};
	},
	
	mounted() {
		this.globalData=getApp().globalData;
		this.list = this.getForm();
		// 判断是否存在 uni-list 组件
		if (this.list) {
			if (!this.list.firstChildAppend) {
				this.list.firstChildAppend = true;
				this.isFirstChild = true;
			}
		
		}
	},
	methods: {
		/**
		 * 获取父元素实例
		 */
		getWord(word,index,index2,index3,index4){
			console.log(word,index,index2);
			this.$emit('getWord',{
				word,index,index2,index3,index4
			});
			
		},
		getForm(name = 'uniList') {
			let parent = this.$parent;
			let parentName = parent.$options.name;
			while (parentName !== name) {
				parent = parent.$parent;
				if (!parent) return false;
				parentName = parent.$options.name;
			}
			return parent;
		},
		onClick(e) {
			if (this.to !== '') {
				this.openPage();
				return;
			}
			if (this.clickable || this.link) {
				this.$emit('click', {
					data: {}
				});
			}
		},
		deleteBook(e){
			this.$emit('deleteBookTap', e.target.dataset);
		},
		insertBook(e){
			this.$emit('insertBookTap', e.target.dataset);
		},
		titleTap(e) {
			this.$emit('titleTap', e.currentTarget.dataset.url);
		},
		title2Tap(e) {
			this.$emit('title2Tap', e.currentTarget.dataset.url);
		},
		subTitle2Tap(e){
			this.$emit('subTitle2Tap', e.currentTarget.dataset.sentence1);	
		},
		subTitle2Tap2(e){
			this.$emit('subTitle2Tap2', e.currentTarget.dataset.sentence2);	
		},
		subTitle2Tap3(e){
			console.log(e);
			this.$emit('subTitle2Tap3', e.currentTarget.dataset.sentence3);
		},
		onSwitchChange(e) {
			this.$emit('switchChange', e.detail);
		},
		openPage() {
			if (['navigateTo', 'redirectTo', 'reLaunch', 'switchTab'].indexOf(this.link) !== -1) {
				this.pageApi(this.link);
			} else {
				this.pageApi('navigateTo');
			}
		},
		pageApi(api) {
			console.log(api);
			console.log(this.to);
			let callback = {
				url: this.to,
				success: res => {
					console.log(res);
					this.$emit('click', {
						data: res
					});
				},
				fail: err => {
					console.log(err);
					this.$emit('click', {
						data: err
					});
				}
			};
			switch (api) {
				case 'navigateTo':
					uni.navigateTo(callback);
					break;
				case 'redirectTo':
					uni.redirectTo(callback);
					break;
				case 'reLaunch':
					uni.reLaunch(callback);
					break;
				case 'switchTab':
					uni.switchTab(callback);
					break;
				default:
					uni.navigateTo(callback);
			}
		}
	}
};
</script>

<style lang="scss">
$uni-font-size-sm: 12px;
$uni-font-size-sm1: 13px;
$uni-font-size-base: 14px;
$uni-font-size-lg: 16px;
$uni-spacing-col-lg: 12px;
$uni-spacing-row-lg: 15px;
$uni-img-size-sm: 20px;
$uni-img-size-base: 26px;
$uni-img-size-lg: 40px;
$uni-border-color: #e5e5e5;
$uni-bg-color-hover: #f1f1f1;
$uni-text-color-grey: #999;
$uni-text-color-yellow: rgba(255, 170, 127, 1);
$list-item-pd: $uni-spacing-col-lg $uni-spacing-row-lg;
.uni-list-item {
	/* #ifndef APP-NVUE */
	display: flex;
	/* #endif */
	font-size: $uni-font-size-lg;
	position: relative;
	justify-content: space-between;
	align-items: center;
	background-color: #fff;
	flex-direction: row;
	/* #ifdef H5 */
	cursor: pointer;
	/* #endif */
}
.uni-list-item--disabled {
	opacity: 0.3;
}
.uni-list-item--hover {
	background-color: $uni-bg-color-hover;
}
.uni-list-item__container {
	position: relative;
	/* #ifndef APP-NVUE */
	display: flex;
	/* #endif */
	flex-direction: row;
	padding: $list-item-pd;
	padding-left: $uni-spacing-row-lg;
	flex: 1;
	overflow: hidden;
	// align-items: center;
}
.container--right {
	padding-right: 0;
}
// .border--left {
// 	margin-left: $uni-spacing-row-lg;
// }
.uni-list--border {
	position: absolute;
	top: 0;
	right: 0;
	left: 0;
	/* #ifdef APP-NVUE */
	border-top-color: $uni-border-color;
	border-top-style: solid;
	border-top-width: 0.5px;
	/* #endif */
}
/* #ifndef APP-NVUE */
.uni-list--border:after {
	position: absolute;
	top: 0;
	right: 0;
	left: 0;
	height: 1px;
	content: '';
	-webkit-transform: scaleY(0.5);
	transform: scaleY(0.5);
	background-color: $uni-border-color;
}
/* #endif */
.uni-list-item__content {
	/* #ifndef APP-NVUE */
	display: flex;
	/* #endif */
	padding-right: 8px;
	flex: 1;
	color: #3b4144;
	// overflow: hidden;
	flex-direction: column;
	justify-content: space-between;
	overflow: hidden;
}
.uni-list-item__content--center {
	justify-content: center;
}
.uni-list-item__content-title {
	font-size: $uni-font-size-lg;
	color: #3b4144;
	overflow: hidden;
}
.uni-list-item__content-subTitle {
	margin-top: 6rpx;
	color: $uni-text-color-grey;
	font-size: $uni-font-size-base;
	// overflow: hidden;
}
.uni-list-item__content-note {
	margin-top: 6rpx;
	color: $uni-text-color-grey;
	font-size: $uni-font-size-sm;
	font-weight: bolder;
	overflow: hidden;
}
.uni-list-item__content-note2 {
	margin-left: 15rpx;
	color: $uni-text-color-yellow;
	font-size: $uni-font-size-sm1;
	font-weight: bolder;
	overflow: hidden;
}
.uni-list-item__extra {
	// width: 25%;
	/* #ifndef APP-NVUE */
	display: flex;
	/* #endif */
	flex-direction: row;
	justify-content: flex-end;
	align-items: center;
}
.uni-list-item__header {
	/* #ifndef APP-NVUE */
	display: flex;
	/* #endif */
	flex-direction: row;
	align-items: center;
}
.uni-list-item__icon {
	margin-right: 18rpx;
	flex-direction: row;
	justify-content: center;
	align-items: center;
}
.uni-list-item__icon-img {
	/* #ifndef APP-NVUE */
	display: block;
	/* #endif */
	height: $uni-img-size-base;
	width: $uni-img-size-base;
	margin-right: 10px;
}
.uni-icon-wrapper {
	/* #ifndef APP-NVUE */
	display: flex;
	/* #endif */
	align-items: center;
	padding: 0 10px;
}
.flex--direction {
	flex-direction: column;
	/* #ifndef APP-NVUE */
	align-items: initial;
	/* #endif */
}
.flex--justify {
	/* #ifndef APP-NVUE */
	justify-content: initial;
	/* #endif */
}
.uni-list--lg {
	height: $uni-img-size-lg;
	width: $uni-img-size-lg;
}
.uni-list--base {
	height: $uni-img-size-base;
	width: $uni-img-size-base;
}
.uni-list--sm {
	height: $uni-img-size-sm;
	width: $uni-img-size-sm;
}
.uni-list-item__extra-text {
	color: $uni-text-color-grey;
	font-size: $uni-font-size-sm;
}
.uni-ellipsis-1 {
	/* #ifndef APP-NVUE */
	overflow: hidden;
	white-space: nowrap;
	text-overflow: ellipsis;
	/* #endif */
	/* #ifdef APP-NVUE */
	lines: 1;
	text-overflow: ellipsis;
	/* #endif */
}
.uni-ellipsis-2 {
	/* #ifndef APP-NVUE */
	overflow: hidden;
	text-overflow: ellipsis;
	display: -webkit-box;
	-webkit-line-clamp: 2;
	-webkit-box-orient: vertical;
	/* #endif */
	/* #ifdef APP-NVUE */
	lines: 2;
	text-overflow: ellipsis;
	/* #endif */
}
</style>
