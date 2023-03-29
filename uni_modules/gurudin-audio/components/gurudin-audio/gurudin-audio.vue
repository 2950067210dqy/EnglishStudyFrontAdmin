<template>
	<view class="gurudin-audio" :class="theme">
    <view class="gurudin-poster">
      <image :src="poster" v-if="poster != ''" class="musicRotate" :class="{'musicPaused':!audio.isPlay}"></image>
      <view class="tools" @click.native="play">
        <image :src="audio.isPlay ? icon.pause : icon.play"></image>
      </view>
    </view>
    <view class="gurudin-content">
      <view class="title">
        {{name}}
        <view class="time">{{formatTime(audio.startTime)}}{{formatTime(audio.duration)=="00:00"?'':'/'+formatTime(audio.duration)}}</view>
      </view>
      <view class="author title" >
	  {{author}}
		  <view class="time" style="display: inline-block;" @click.native="reload">
				<uni-icons type="reload" color="grey" size="20"></uni-icons>
			</view>
	  
	  </view>
	  	<!-- <progress style="margin-bottom: 10rpx;" :percent="audio.playProgress" activeColor="#10AEFF" stroke-width="5" /> -->
		<slider style=""  :value="audio.playProgress" @change="sliderChange"  :block-size="12"/>
    </view>
	</view>
</template>

<script>
	
	export default {
		name:"gurudin-audio",
emits: ['click'],
    props: {
      /**
       * 播放回调方法
       */
      call: {
        type: Function
      },

      /**
       * 封面
       */
      poster: {
        type: String,
        default: ''
      },

      /**
       * 演唱者
       */
      author: {
        type: String,
        default: ''
      },

      /**
       * 歌曲名称
       */
      name: {
        type: String,
        default: ''
      },

      /**
       * 音频链接
       */
      src: {
        type: String,
        required: true
      },

      /**
       * 主题
       */
      theme: {
        type: String,
        default: 'light'
      },

      /**
       * 是否循环播放
       */
      loop: {
        type: Boolean,
        default: false
      },

      /**
       * 是否遵循系统静音开关
       */
      obeyMuteSwitch: {
        type: Boolean,
        default: true
      },
    },

		data() {
			return {
        /**
         * icon
         */
        icon: {
          play: 'data:image/png;base64,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',
          pause: 'data:image/png;base64,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',
        },

        /**
         * 音频播放对象
         */
        audio: {
          /**
           * 是否正在播放
           * false=否 true=是
           */
          isPlay: false,
          
          /**
           * 当前播放时间
           */
          startTime: 0,
          
          /**
           * 音频总时长
           */
          duration: 0,
          
          /**
           * 播放进度
           */
          playProgress: 0,
          
          /**
           * 音频播放对象
           */
          innerAudioContext: null,
        },
		 timer:null,
		 timeFunc:()=>{
	
			 this.audio.startTime+=1;
		 }
		};
		},

    methods: {
      /**
       * 格式化播放时间 00:00
       */
      formatTime(second) {
		  
        if (second == 0||second==Infinity) {
          return '00:00';
        }
        
        let mm = Number((second / 60).toFixed(0));
        mm = mm < 10 ? '0' + mm : mm;

        let ss = Number((second % 60).toFixed(0));
        ss = ss < 10 ? '0' + ss : ss;

        return mm + ':' + ss;
      },
	  //拖动进度
	  sliderChange(e){
		  console.log(e);
		  this.audio.isPlay=false;
		  this.audio.playProgress=e.detail.value;
		  
		  // this.audio.playProgress = (Number(this.audio.startTime=.toFixed(2)) / this.audio.duration * 100).toFixed(2);
		  this.audio.startTime=(this.audio.playProgress/100).toFixed(2) *this.audio.duration
		  // this.audio.startTime=(this.audio.duration )* this.audio.playProgress* 100 ;
		  this.audio.innerAudioContext.pause();  // 先暂停
		  if(this.timer!=null){
		  	  console.log("pause",this.timer);
		  	  clearInterval(this.timer);
		    }
		  this.audio.innerAudioContext.offCanplay();
		  this.audio.innerAudioContext.offPlay();
		  this.audio.innerAudioContext.offPause();
		  this.audio.innerAudioContext.offError();
		  this.audio.innerAudioContext.offTimeUpdate();
		     this.audio.innerAudioContext.seek(this.audio.startTime); // 再跳转
		      setTimeout(() => {
		       this.play()
		      }, 500);   
	  },
	  
		//重新播放
		reload(){
			if (this.audio.innerAudioContext != null) {
				this.audio.isPlay=false;
			
				this.audio.playProgress=0;
				 this.audio.startTime=0;
				this.audio.innerAudioContext.pause()  // 先暂停
				if(this.timer!=null){
					  console.log("pause",this.timer);
					  clearInterval(this.timer);
				  }
				this.audio.innerAudioContext.offCanplay();
				this.audio.innerAudioContext.offPlay();
				this.audio.innerAudioContext.offPause();
				this.audio.innerAudioContext.offError();
				this.audio.innerAudioContext.offTimeUpdate();
				   this.audio.innerAudioContext.seek(0) // 再跳转
				    setTimeout(() => {
				     this.play()
				    }, 500);   
			}
		},
      /**
       * 播放 or 暂停
       */
      play() {
		  if (this.audio.isPlay) {
			this.audio.isPlay=false;
				this.$emit('click', {audioContext:this.audio.innerAudioContext,isPlay:this.audio.isPlay,this:this});
				this.pauseFormat();
		   
		  } else {
			  	this.audio.isPlay=true;
					this.$emit('click', {audioContext:this.audio.innerAudioContext,isPlay:this.audio.isPlay,this:this});
					this.playFormat();
		  }
	
   //      if (this.audio.isPlay) {
			
   //        // 暂停播放
   //        this.onPause();
   //      } else {
   //        // 开始播放
   //        if (typeof this.call == 'function') {
			 
   //          this.call(this.src);
   //        }
          
   //        this.onPlay();
   //      }
      },
	playFormat(){
		// 开始播放
		if (typeof this.call == 'function') {
		 
		  this.call(this.src);
		}
		
		this.onPlay();
	},
	pauseFormat(){
		// 暂停播放
		this.onPause();
	},
      /**
       * 暂停
       */
      onPause() {
		  
        if (this.audio.innerAudioContext != null) {
          this.audio.isPlay = false;
          this.audio.innerAudioContext.onPause(()=>{
			  if(this.timer!=null){
				  console.log("pause",this.timer);
				  clearInterval(this.timer);
			  }
          	this.audio.innerAudioContext.offCanplay();
          	this.audio.innerAudioContext.offPlay();
          	this.audio.innerAudioContext.offPause();
          	this.audio.innerAudioContext.offError();
          	this.audio.innerAudioContext.offTimeUpdate();
          })
          this.audio.innerAudioContext.pause();
        }
		  // this.$emit('click', {audioContext:this.audio.innerAudioContext,isPlay:this.audio.isPlay,this:this});
      },

      /**
       * 播放
       */
      onPlay() {
        this.audio.isPlay = true;
		let player=this.audio.innerAudioContext;
		let flag =true;
        if (this.audio.innerAudioContext == null) {
			player = uni.createInnerAudioContext();
			 player.src = this.src; 
			 player.loop=this.loop;
			 player.obeyMuteSwitch= this.obeyMuteSwitch;
			 player.autoplay=this.autoplay;
				
		
        }else{
			flag=false;
		}
		// console.log(player);
		if(flag){
			player.onCanplay(() =>{
				
			  this.audio.duration=player.duration;
			  
			  // this.audio.innerAudioContext.onCanplay(loadDuration());
			
			   player.play();
			  // 音频播放进度更新事件
			  player.onTimeUpdate(() =>{
			  	
			    if (this.audio.duration == 0) {		     
					  this.audio.duration= player.duration;
				 
			    } else {
			      this.audio.playProgress = (Number(this.audio.startTime.toFixed(2)) / this.audio.duration * 100).toFixed(2);
			    }
			    // console.log(player.currentTime);
			     this.audio.startTime = player.currentTime;
			  });
			  
			  // 播放完成事件
			  player.onEnded(() =>{
			    this.audio.startTime = 0;
			    this.audio.playProgress = 0;
			    
			    if (!this.loop) {
			      this.onPause();
			    }
			  });
			  
			  // 音源错误
			  player.onError((res) => {
			    this.audio.isPlay = false;
			  
			    uni.showModal({
			      title: '提示',
			      content: '系统错误',
			      showCancel: false
			    });
			  });
			  this.audio.innerAudioContext=player;
			  // this.$emit('click', {audioContext:this.audio.innerAudioContext,isPlay:this.audio.isPlay,this:this});
			});
		}else{
			
		
			 player.play();
			player.onPlay(()=>{
				if(this.timer!=null){
						clearInterval(this.timer);
				}
				this.timer = setInterval(this.timeFunc,1000);
				
				console.log("play",this.timer);
			});
			
			// 音频播放进度更新事件
			player.onTimeUpdate(() =>{
				
			  if (this.audio.duration == 0) {
				  // console.log(1);
				   this.audio.duration= player.duration;
				
			    // this.audio.duration =player.duration;
			  } else {

			    this.audio.playProgress = (Number(this.audio.startTime.toFixed(2)) / this.audio.duration * 100).toFixed(2);
			  }
			  
			  // this.audio.startTime =this.audio.startTime+0.25;
			});
			
			// 播放完成事件
			player.onEnded(() =>{
			  this.audio.startTime = 0;
			  this.audio.playProgress = 0;
			  
			  if (!this.loop) {
				 if(this.timer!=null){
					  clearInterval(this.timer);
				 }
			    this.onPause();
			  }
			});
			
			// 音源错误
			player.onError((res) => {
			  this.audio.isPlay = false;
				if(this.timer!=null){
						clearInterval(this.timer);
				}
			  uni.showModal({
			    title: '提示',
			    content: '系统错误',
			    showCancel: false
			  });
			});
			this.audio.innerAudioContext=player;
			// this.$emit('click', {audioContext:this.audio.innerAudioContext,isPlay:this.audio.isPlay,this:this});
		}
		
		  
       
		
      },
    },

    created() {
     
    }
	}
</script>

<style lang="scss">
.light {
  border: 2rpx solid #e8eaec;
  background: #fff;

  .gurudin-poster {
    background: #dcdee2;
  }

  .gurudin-content {
    .title {
      color: #2C405A;

      .time {
        color: #808695;
      }
    }

    .author {
      color: #808695;
    }
  }
}

.dark {
  border: 2rpx solid #17233d;
  background: #515a6e;

  .gurudin-poster {
    background: #17233d;
  }

  .gurudin-content {
    .title {
      color: #FFFFFF;

      .time {
        color: #c5c8ce;
      }
    }

    .author {
      color: #c5c8ce;
    }
  }
}

.gurudin-audio {
  animation:mymove .5s;
  position: relative;
  height: 130rpx;
  border-radius: 8rpx;
  clear: both;

  .gurudin-poster, .gurudin-content {
    float: left;
  }

  .gurudin-poster {
    position: relative;
    width: 130rpx;
    height: 126rpx;
    margin-top: 1rpx;
    margin-left: 1rpx;
    border-top-left-radius: 8rpx;
    border-bottom-left-radius: 8rpx;

    image {
      width: 110rpx;
      height: 110rpx;
      margin: 10rpx;
      opacity: 0.8;
      border-radius: 100%;
    }

    .tools {
      position: absolute;
      text-align: center;
      top: 0;
      left: 0;
      height: 130rpx;
      width: 130rpx;

      image {
        width: 50rpx;
        height: 50rpx;
        margin-top: calc((100% - 50rpx) / 2);
      }
    }
  }

  .gurudin-content {
    width: calc(100% - 130rpx - 62rpx);
    height: calc(130rpx - 60rpx);
    padding: 5rpx 30rpx 5rpx 30rpx;

    .title {
      position: relative;
      font-size: 28rpx;
      height: 30rpx;

      .time {
        position: absolute;
        right: 0;
        top: 0;
        font-size: 24rpx;
      }
    }

    .author {
      margin-top: 10rpx;
      font-size: 24rpx;
    }
  }
}

@keyframes mymove
{
	from {
    opacity: 0;
  }

	to {
    opacity: 1;
  }
}

// 旋转动画
.musicRotate{
  animation: rotate 10s linear infinite;
}
  
@keyframes rotate{
  from{
    transform: rotate(0deg)
  }
  to{
    transform: rotate(359deg)
  }
}
  
.musicPaused{
  animation-play-state: paused;
}
</style>
