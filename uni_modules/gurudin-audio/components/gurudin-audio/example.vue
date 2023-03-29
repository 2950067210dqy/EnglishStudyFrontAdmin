<template>
	<view class="content">

    <view v-for="(audio, index) in list" :key="index">
      <gurudin-audio
          :src="audio.src"
          :poster="audio.poster"
          :name="audio.name"
          :author="audio.author"
          :call="playCall"
          :theme="index == 0 ? 'light' : 'dark'"
          ref="audioPlayers"
        ></gurudin-audio>
        
      <view :style="{height: '100rpx'}"></view>
    </view>

	</view>
</template>

<script>
  let vm = null

	export default {
		data() {
			return {
        list: [
          {
            src: 'https://whizme-assets.8686c.com/webapi-assets-prod/resources/banners/202108/mp3_url.ylx3d67fwrz7r02l.mp3',
            poster: 'https://whizme-assets.8686c.com/webapi-assets-prod/resources/banners/202108/pic.zm48z9bk9440dr6k.jpeg',
            name: '你的眼晴背叛了你的心',
            author: '鄭中基'
          },
          {
            src: 'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-hello-uniapp/2cc220e0-c27a-11ea-9dfb-6da8e309e0d8.mp3',
            poster: 'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/7fbf26a0-4f4a-11eb-b680-7980c8a877b8.png',
            name: '致爱丽丝',
            author: '路德维希·范·贝多芬'
          },
        ],
			}
		},
		
		methods: {
      /**
       * 点击播放回调方法
       */
      playCall(src) {
        vm.$refs.audioPlayers.forEach(e =>{
          if (src != e.src) {
            e.onPause();
          }
        });
      },
		},

    created() {
      vm = this;
    }
	}
</script>

<style>
  page {
    /* background-color: #202124; */
    padding: 100rpx 0;
  }
  
  .content {
	  padding: 0 30rpx;
  }
</style>
