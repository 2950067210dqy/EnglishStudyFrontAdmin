<template>
	<view style="border-radius: 20rpx;">
	<canvas :style="{'width':width+'rpx','height':height+'rpx'}" :canvas-id="id" :id="id"></canvas>
	</view>
</template>

<script>
	import { getCurrentInstance } from 'vue' // 引入
	

	export default {
		name: 'myNavBack',
		emits: ['click'],
		props: {
			width: {
				type: String,
				default: '100'
			},
			height: {
				type: String,
				default: '100'
			},
			id:{
				type: String,
				default: ''
			},
			text:{
				type: String,
				default: 'LJR'
			}

		},
		computed:{
			
		},
		mounted() {
			this.get_text_img();
		},
		updated() {
			this.get_text_img();
		},
		methods: {
			get_text_img() {
				
				let colors = ["rgb(239,150,26)", 'rgb(152, 34, 121)', "rgb(111,75,255)", "rgb(36,174,34)", "rgb(80,80,80)", "rgb(148, 0, 74)", "rgb(94, 140, 69)", "rgb(85, 0, 0)", "rgb(149, 150, 82)"];
				let random =getApp().done(1,colors.length);
				//在组件中uni.createCanvasContext()方法的第二个参数为this指向错误，重新指明this
				const instance = getCurrentInstance() ;
				// 这里是创建 canvas 绘图上下文
				let ctx = uni.createCanvasContext(this.id,instance);
				ctx.setFillStyle(colors[random] );
				ctx.fillRect(0,0,this.width,this.height);
				
				// this.fontSize=this.width/(this.text.length+1);
			
				
				ctx.setFillStyle('white')
				ctx.setTextAlign('center');
			
				if(this.text.length>8){
					this.fontSize=this.width/10.8;
					ctx.setFontSize(this.fontSize);
					ctx.fillText(this.text.substring(0,4),this.width/3.5,this.fontSize*2.5);
					
					ctx.fillText(this.text.substring(4,8),this.width/3.5,this.fontSize*4.0);
					ctx.fillText(this.text.substring(8,12),this.width/3.5,this.fontSize*5.6);
				}else if(this.text.length>4&&this.text.length<=8){
						this.fontSize=this.width/10;
					ctx.setFontSize(this.fontSize);
					ctx.fillText(this.text.substring(0,4),this.width/3.5,this.fontSize*2.7);
					
					ctx.fillText(this.text.substring(4,8),this.width/3.5,this.fontSize*4.5);
				}else{
						this.fontSize=this.width/9.6;
					ctx.setFontSize(this.fontSize)
					ctx.fillText(this.text.substring(0,4),this.width/3.5,this.height/4);
				}
				
				ctx.draw();
			  },
			onClick(type) {
				this.$emit('click', type)
			}
		}
	}
</script>

<style>

</style>
