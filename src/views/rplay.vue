<template>
  <video-player class="vjs-custom-skin"
    ref="videoPlayer"
    :options="playerOptions"
    :playsinline="true"
    customEventName="customstatechangedeventname">
  </video-player>
</template>

<script>
import 'videojs-flash'
export default {
  name: 'rtmp-live',
  props: {
    src: {
      type: String,
      default: 'rtmp://live.hkstv.hk.lxdns.com/live/hks2'
    }
  },
  data () {
    return {
      playerOptions: {
		height: '340',
        width: '100%',
        sources: [ {
          type: 'rtmp/mp4',
          src: this.src
        } ],
        techOrder: ['flash'],
        autoplay: false, // 非自动播放
        // controls: true, // 控制条
        isVideo:true,//强制刷新使用
        // preload: 'auto', // 预加载
        // muted: true, // 消除所有音频
        // loop: false, // 循环播放
         poster: 'https://surmon-china.github.io/vue-quill-editor/static/images/surmon-9.jpg' //首屏图片
      }
    }
  }
}
</script>