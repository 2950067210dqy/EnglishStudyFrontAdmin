<template>
	<view v-if="controls" class="_contain">
    <!-- 海报和按钮 -->
		<view class="_poster" :style="'background-image:url('+poster+')'">
			<view class="_button" @tap="_buttonTap">
				<view :class="playing?'_pause':'_play'" />
			</view>
		</view>
    <!-- 曲名和作者 -->
		<view class="_title">
			<view class="_name">{{name||'未知音频'}}</view>
			<view class="_author">{{author||'未知作者'}}</view>
		</view>
    <!-- 进度条 -->
    <slider class="_slider" activeColor="#585959" block-size="12" handle-size="12" :disabled="error" :value="value" @changing="_seeking" @change="_seeked" />
    <!--播放时间-->
		<view class="_time">{{time||'00:00'}}</view>
	</view>
</template>

<script>
/**
 * @fileoverview audio 组件
 */
import context from './context'
export default {
	emits: ['click'],
  data () {
    return {
		id:'',
      error: false,
      playing: false,
      time: '00:00',
      value: 0
    }
  },
  props: {
    aid: String,
    name: String, // 音乐名
    author: String, // 作者
    poster: String, // 海报图片地址
    autoplay: [Boolean, String], // 是否自动播放
    controls: [Boolean, String], // 是否显示控件
    loop: [Boolean, String], // 是否循环播放
    src: String // 源地址
  },
  watch: {
    src (src) {
      this.setSrc(src)
    }
  },
  mounted () {
	this.id=getApp().globalData.Base64.encode(getApp().globalData.getDateTime(1)+getApp().globalData.done(5,10));
    this._ctx = uni.createInnerAudioContext()
    this._ctx.onError((err) => {
      this.error = true
      this.$emit('error', err)
    })
    this._ctx.onTimeUpdate(() => {
      const time = this._ctx.currentTime
      const min = parseInt(time / 60)
      const sec = Math.ceil(time % 60)
      this.time = (min > 9 ? min : '0' + min) + ':' + (sec > 9 ? sec : '0' + sec)
      if (!this.lastTime) {
        this.value = time / this._ctx.duration * 100 // 不在拖动状态下
      }
    })
    this._ctx.onEnded(() => {
      if (!this.loop) {
        this.playing = false
      }
    })
    context.set(this.aid, this)
    this.setSrc(this.src)
  },
  beforeDestroy () {
    this._ctx.destroy()
    context.remove(this.aid)
  },
  onPageShow () {
    if (this.playing && this._ctx.paused) {
      this._ctx.play()
    }
  },
  methods: {
    // 设置源
    setSrc (src) {
      this._ctx.autoplay = this.autoplay
      this._ctx.loop = this.loop
      this._ctx.src = src
      if (this.autoplay && !this.playing) {
        this.playing = true
      }
	
    },
    // 播放
    play () {
      this._ctx.play()
      this.playing = true
      this.$emit('play', {
        target: {
          id: this.aid
        }
      })
    },
    // 暂停
    pause () {
      this._ctx.pause()
      this.playing = false
      this.$emit('pause')
    },
    // 设置播放速率
    playbackRate (rate) {
      this._ctx.playbackRate = rate
    },
    // 移动进度条
    seek (sec) {
      this._ctx.seek(sec)
    },
    // 内部方法
    _buttonTap () {
		this.$emit('click', {playing:this.playing,id:this.id});
      if (this.playing) this.pause()
      else this.play()
    },
    _seeking (e) {
      // 避免过于频繁 setData
      if (e.timeStamp - this.lastTime < 200) return
      const time = Math.round(e.detail.value / 100 * this._ctx.duration)
      const min = parseInt(time / 60)
      const sec = time % 60
      this.time = (min > 9 ? min : '0' + min) + ':' + (sec > 9 ? sec : '0' + sec)
      this.lastTime = e.timeStamp
    },
    _seeked (e) {
      this.seek(e.detail.value / 100 * this._ctx.duration)
      this.lastTime = undefined
    }
  }
}
</script>

<style>
/* 顶层容器 */
._contain {
  position: relative;
  display: inline-flex;
  width: 500rpx;

  background-color: #fcfcfc;
  border: 1px solid #e0e0e0;
  border-radius: 2px;
}

/* 播放、暂停按钮 */
._button {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 50rpx;
  height: 50rpx;
  overflow: hidden;
  background-color: rgb(0, 0, 0, 0.2);
  border: 1rpx solid white;
  border-radius: 50%;
  opacity: 0.9;
}

._play {
  margin-left: 2rpx;
  border-top: 7rpx solid transparent;
  border-bottom: 7rpx solid transparent;
  border-left: 12rpx solid white;
}

._pause {
  width: 12rpx;
  height: 12rpx;
  background-color: white;
}

/* 海报 */
._poster {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 150rpx;
  height: 150rpx;
  background-color: #e6e6e6;
  background-size: contain;
}

/* 标题栏 */
._title {
  flex: 1;
  margin: 5rpx 0 0 20rpx;
  text-align: left;
}

._author {
	margin-top: 45rpx;
  width: 100%;
  font-size: 24rpx;
  color: #888;
}

._name {
  width: 140rpx;
  font-size: 27rpx;
  line-height: 39rpx;
}

._author,
._name {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

/* 进度条 */
._slider {
  position: absolute;
  right: 16rpx;
  bottom: 8rpx;
  width: 280rpx;
  margin: 0;
}

/* 播放时间 */
._time {
  margin: 7rpx 14rpx 0 0;
  font-size: 25rpx;
  color: #888;
}


</style>
