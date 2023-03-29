<template>
	<view class="main">
		<my-navBack backColor="white"  background="linear-gradient(90deg, #0d0d0d, #595959)" title="充值"></my-navBack>
		<view class="header">	
			
			
		</view>
		<view class="body">
			<view class="content"> 
				<web-view  v-if="src!=''"  :src="src"></web-view>
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
				webviewStyles: {
									progress: true,
									with:"200rpx",
									height:"200rpx"
				},
				src:'',
			}
		},
		onLoad(param) {
			uni.showLoading({
				title:'加载中'
			});
			
			this.globalData=getApp().globalData;
			this.src=decodeURIComponent(param.src);
			setTimeout(()=>{
				uni.hideLoading();
			},5000)
		},
		onShow() {
			this.globalData=getApp().globalData;
			if(this.globalData.checkLogin()){
				this.loadData();
			}
		},
		onReady() {
			
		},
		methods: {
			//加载服务器数据 start
			loadData(){
			
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
