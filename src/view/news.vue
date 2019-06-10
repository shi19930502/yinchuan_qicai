<template>
	<div class="news">
		<van-nav-bar title="实时动态" right-text="首页" @click-right="$root.onClickRight" left-text="返回" left-arrow @click-left="$root.onClickLeft" />
		<div class="nav_div"></div>
		<div class="news_content" >
			<div class="title">{{newsObj.title}}</div>
			<div class="header">
				<img v-if='isJinHe' src="../assets/img/icon_logo.png" alt="" />
				<div class="right">
					<div class="right_1">{{titleName}}</div>
					<div class="right_2">{{newsObj.author}}&nbsp;|&nbsp;{{newsObj.publishTime}}</div>
				</div>
			</div>
			<div class="html_content"  v-html="newsObj.content">{{newsObj.content}}</div>
		</div>
		<div class="footer">
	    	<p class='footerName'>宁夏杞彩回乡土特产开发有限公司</p>
	    	<!--<p class="jszc">技术支持：成都九洲电子信息系统股份有限公司</p>-->
	    </div>
	</div>
</template>

<script>
	import Vue from 'vue'
	import "./../scss/news.scss"
	import { NavBar, Toast } from 'vant';
	var componentsArr = [NavBar, Toast ]
	Vue.use(function(Vue) {
		componentsArr.map(component => {
			Vue.component(component.name, component);
		});
		for(var key in componentsArr[0].components) {
			Vue.component(componentsArr[0].components[key].name, componentsArr[0].components[key]);
		}
	})
	export default{
		data(){
			return {
				title:TITLE,
				newsObj:{
					title:'',
					author:'',
					content:'',
					cover:'',
					publishTime:'',
				},
				titleName:'',
				isJinHe:false,
			}
		},
		mounted(){
			if(document.documentElement && document.documentElement.scrollTop) {
				document.documentElement.scrollTop = document.body.scrollTop = 0;
			} else if(document.body) {
				document.body.scrollTop = 0;
			}
			if(TITLE=='宁夏金河科技股份有限公司'){
				this.isJinHe=true
				this.titleName='金河乳业'
			}else if(TITLE=='宁夏广玉面粉有限公司'){
				this.titleName='广玉面粉'
			}
       		document.documentElement.style.fontSize = '16px';
			this.init()
		},
		methods:{
			init(){
				this.$root.ajax({
					name:  "news/getVisitCount/"+this.$route.query.newsId,
					type: 'get',
				}).then((d) => {
					this.newsObj=d.result
				})
			},
		},
		destroyed(){
			 var winWidth = window.innerWidth;
        	// console.log(winWidth);
        	document.documentElement.style.fontSize = winWidth / 750 * 100 + 'px';
		}
		
	}
</script>

<style>
</style>