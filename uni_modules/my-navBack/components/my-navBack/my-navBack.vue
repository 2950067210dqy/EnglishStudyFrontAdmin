<template>
	
	<view class="Top" :style="{'background':background}">
			   <navigator :url="url" style="display: inline-block;margin-left: 20rpx;" :open-type="type"  hover-class="navigator-hover">
			   	<uni-icons type="back" :color="backColor" size="20"></uni-icons>
			   </navigator>	
			<view :style="`font-size: ${fontSize};display: inline-block;width:80%`">
				<view style=" width:100%;display: flex;justify-content:center;flex-direction: row;">
					<view class="left" style="flex: 2;">
						<slot name="left">
							
						</slot>
					</view>
					<view class="cotent"   :style="{'flex':'auto','color':backColor}">
						{{title}}
					</view>
					<view class="right"  style="flex: auto;">
						<slot name="right">
							
						</slot>
					</view>
				</view>	
				<!-- <view style="display: flex;justify-content:center; width:100%;">
					
				</view> -->	
			</view>
	</view>
</template>

<script>
	export default {
		name: 'myNavBack',
		emits: ['click'],
		props: {
			title: {
				type: String,
				default: '默认标题'
			},
				fontSize: {
				type: String,
				default: '40rpx'
			},
		
			backColor:{
				type: String,
				default: 'black'
			},
			background :{
				type: String,
				default: 'white'
			},
			type: {
				type: String,
				default: 'navigateBack'
			},
			url: {
				type: String,
				default: ''
			},
		},
	
		methods: {
			onClick(type) {
				this.$emit('click', type)
			}
		}
	}
</script>

<style>
	.Top{
		padding-left: 10rpx;
		
		/* background-color: white; */
		padding-top: 60rpx;
	}
</style>