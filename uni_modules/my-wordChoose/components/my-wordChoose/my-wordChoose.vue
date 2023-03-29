<template>
	<view>
			<uni-data-picker placeholder="选则单词" popup-title="请选择所在地区" :localdata="wordTree" :modelValue="chooseWord"
									@change="onchange" @nodeclick="onnodeclick" >
			</uni-data-picker>
	</view>
</template>

<script>
	export default {
		name: 'myWordChoose',
		emits: ['change','nodeclick'],
		
		props: {
			chooseWord: {
				type: String,
				default: ''
			},
			wordTree:{
				type:Array,
				default:[]
			}
		},
		data() {
			return {
				globalData: {},
				
			};
		},
		mounted() {
			
			
		},
		methods: {
		
			onchange(e){
				this.$emit('change',e)
			},
			onnodeclick(e){
				this.$emit('nodeclick',e)
			},
		}
	}
</script>

<style>
</style>