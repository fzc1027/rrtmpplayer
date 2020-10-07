<template>
	<div>
		<videoPlayer class="vjs-custom-skin videoPlayer" :options="playerOptions"></videoPlayer>
	</div>
</template>
<script>
import 'video.js/dist/video-js.css'
import {videoPlayer} from 'vue-video-player'
import 'videojs-flash'

export default {
	components: {
		videoPlayer
	},
	data () {
	return {
		playerOptions: {
			height: '300',
			sources: [{
				type: 'rtmp/mp4',
				src: 'rtmp://58.200.131.2:1935/livetv/hunantv'
			}],
			techOrder: ['flash'],
			autoplay: false,
			controls: true
			}
		}
	}
}
</script>
