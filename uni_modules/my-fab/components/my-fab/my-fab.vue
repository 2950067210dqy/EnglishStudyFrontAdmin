<template>
	<view  class="fab">
		<uni-transition  ref="ani" :mode-class="['fade', 'slide-right']" :show="!isShowFabBtn">
			<uni-fab @blur="onblur"
				tabindex="-1"
				:pattern="fixButtonPattern"
				:content="fixButtonContent"
				horizontal="right"
				vertical="bottom"
				direction="vertical"
				@trigger="fabBtnTrigger"
				@fabClick=""
			></uni-fab>
		</uni-transition>
		<uni-transition ref="ani" :mode-class="[ 'slide-right']" :show="isShowFabBtn">
			<view class="fabOpen" @tap="triggerShowFabBtn">{{  text1}}</view>
		</uni-transition>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				globalData:{},
				fixButtonContent: [
					{
						iconPath: '../../../../static/fabBtn/newVar.png',
						selectedIconPath: '../../../../static/fabBtn/newVar_selected.png',
						text: '生词本 ',
						active: false
					},
					{
						iconPath: '../../../../static/fabBtn/hidde.png',
						selectedIconPath: '../../../../static/fabBtn/hidde_selected.png',
						text: '隐藏',
						active: false
					}
				],
				fixButtonPattern: {
					color: '#626262',
					selectedColor: 'rgba(255, 170, 127, 1)',
					backgroundColor: '#ffffff',
					buttonColor: 'rgba(255, 170, 127, 1)'
				},
				isShowFabBtn: true,
				text1:'点击展开',
			}
		},
		onLoad() {
		
		},
		onShow() {
			
		},
		onReady() {
			
		},
		methods: {
			//失去焦点 start
			onblur(){
				console.log("asdasdasdasdasd");
			},
			//失去焦点 end
			//加载服务器数据 start
			loadData(){
			
			},
			//加载服务器数据 end
			//显示固定按钮 start
			triggerShowFabBtn() {
				this.isShowFabBtn = !this.isShowFabBtn;
				console.log(this.isShowFabBtn);
			},
			//显示固定按钮 end
			//点击固定按钮的展开按钮 start
			fabBtnTrigger(e) {
				this.fixButtonContent.forEach(item => {
					if (item.text === e.item.text) {
						item.active = !e.item.active;
					}
					return item;
				});
				switch (e.index) {
					case 0: //前往生词本
						uni.navigateTo({
							url:"/pages/newWordBook/newWordBook"
						});
					case 1:
						this.fixButtonContent.forEach(item => {
							item.active = false;
							return item;
						});
						this.triggerShowFabBtn();
						break;
				}
			}
			//点击固定按钮的展开按钮 end
		}
	}
</script>

<style>
	.fab{
		bottom: 10rpx;
		right: 0rpx;
		z-index: 5;
		position: fixed;
	}
.fabOpen {
	border-radius: 5rpx;
	padding-left: 20rpx;
	padding-top: 10rpx;
	padding-bottom: 10rpx;
	
	background-color: #ffaa7f;
	
	font-size: 20rpx;
	width: 30rpx;
	height: 100rpx;
	color: white;
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
