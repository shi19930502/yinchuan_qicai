<template>
	<div class="monitoring">
		<van-nav-bar title="实时监控" right-text="首页" @click-right="$root.onClickRight" left-text="返回" left-arrow @click-left="$root.onClickLeft" />
		<div class="nav_div"></div>
		<div class="content">
			<!--<video width="800" height="" poster="../assets/img/video.png" controls="controls" >
				<source src="https://vjs.zencdn.net/v/oceans.mp4" type="video/mp4"></source>
				<source src="myvideo.ogv" type="video/ogg"></source>
				<source src="myvideo.webm" type="video/webm"></source>
				<object width="" height="" type="application/x-shockwave-flash" data="myvideo.swf">
					<param name="movie" value="myvideo.swf" />
					<param name="flashvars" value="autostart=true&amp;file=myvideo.swf" />
				</object>
				当前浏览器不支持 video直接播放，点击这里下载视频：
				<a href="myvideo.webm">下载视频</a>
			</video>-->
			<template v-if='dataList&&dataList.length>0'>
				<div class="content_list" v-for='(item,index) in dataList'>
					<video  id="videoCooked" poster="../assets/img/video.png" controls="controls">
						<source :src="item.src" type="video/mp4" />
						<source id="ogg_src" src="https://vjs.zencdn.net/v/oceans.mp4" type="video/ogg"> 您的浏览器不支持 HTML5 video 标签。
					</video>
					<p class="title">{{item.name}}</p>
					<p class="name">描述：{{item.desc}}</p>
				</div>
			</template>
			<div v-else class="no_video" >暂无视频</div>
			
		</div>
	</div>
</template>

<script>
	import { NavBar, Toast } from 'vant';
	export default {
		components: {
			[NavBar.name]: NavBar
		},
		data() {
			return {
				dataList:[],
			}
		},
		computed: {

		},
		mounted() {
			this.init()
		},
		methods: {
			init() {
				var name = 'video/visible/0'
				if(this.$route.query.str == 1) {
					name = 'video/visible/1'
				}
				this.$root.ajax({
					//url: this.$root.config.api_url + "traceablity/getTraceablityInfo",
					name: name,
					type: 'get',
				}).then((d) => {
					console.log(d)
					if(d.status == 'success' && typeof d.result != 'string') {
						this.dataList=d.result||[]
					}
				})
			}
		}
	}
</script>

<style lang="scss">
	@import './../scss/monitoring.scss';
</style>