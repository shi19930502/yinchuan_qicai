<template>
	<div class="index">
		<!--:style='bannerStyle'-->
		<div class="header">
			<!--<van-notice-bar  @click='noticeClick' :text="newsEnityList[0].title" :left-icon="gb_logo" />-->
			<div class="header_animation">
				<div class="header_img_1_f">
					<img class="header_img_1" src="../assets/img/logo.png" alt="" />
				</div>
				<p class="header_p_1">专注枸杞十五年，只做地道好枸杞</p>
				<img class="header_img_2" src="../assets/img/by.png" alt="" />
				<p class="header_p_2">宁夏杞彩回乡土特产开发有限公司</p>
			</div>
		</div>
		<!--当有基地的时候-->
		<div class="header_nav " :class="{header_nav_base:farmEntityView}">
			<router-link :to="{name:'enterprise',query:{traceablityNo:$route.query.traceablityNo}}">
				<div class="header_nav_list">
					<p>企业信息</p>
				</div>
			</router-link>
			<router-link v-if='farmEntityView' :to="{name:'base',query:{traceablityNo:$route.query.traceablityNo}}">
				<div class="header_nav_list">
					<p>基地信息</p>
				</div>
			</router-link>
			<router-link :to="{name:'client',query:{traceablityNo:$route.query.traceablityNo,varietyId:varietyEntityView.varietyId,varietySpecId:varietyEntityView.varietySpecId}}">
				<div class="header_nav_list">
					<p>评价反馈</p>
				</div>
			</router-link>
			<router-link :to="{name:'monitoring',query:{str:1,traceablityNo:$route.query.traceablityNo}}">
				<div class="header_nav_list">
					<p @click='goMonitoring(0)'>实时监控</p>
				</div>
			</router-link>
		</div>
		<div v-if='isNoticeShow' class="notice">
			<van-notice-bar  @click='noticeClick' :text="newsEnityList[0].title" :left-icon="gb_logo" />
		</div>
		<div class="content" v-if='isContentShow'>
			<div class="product_info">
				<div class="box_header">
					<div class="box_header_name">产品信息</div>
				</div>
				<div class="p_i_content">
					<div class="name">{{varietyEntityView.breedName}}</div>
					<div class="num">
						<span class="num_1">追溯码:</span>
						<span class="num_2">{{$route.query.traceablityNo}}</span>
					</div>
					<img v-if='varietyEntityView.photo&&varietyEntityView.photo!=null' :src="varietyEntityView.photo" alt="" />
					<ul>
						<li>基础信息</li>
						<li>商家：{{company.name}}</li>
						<li>品种：<span>{{varietyEntityView.breedName}}</span></li>
						<li>规格：<span>{{varietyEntityView.specifications}}</span></li>
						<li>等级：<span>{{varietyEntityView.gradeName}}</span></li>
						<li v-if='varietyEntityView.organicCropsName=="是"'>是否有机：<span>{{varietyEntityView.organicCropsName}}</span></li>
						<li>生产时间：<span>{{varietyEntityView.production}}</span></li>
						<li>过期时间：<span>{{varietyEntityView.expirationDate}}</span></li>
					</ul>
				</div>

				<div class="jcbg" v-if='productInforShow'>
					<h1>检测报告</h1>
					<div class="h_i_content">

						<div class="h_i_c_bottom">
							<div class="h_i_c_bottom_center">
								<div>检测结果：<span class="hege">{{productInfor.result}}</span></div>
								<div>检测标准：<span>{{productInfor.inspectionStandardName}}</span></div>
								<div>检测人：<span>{{productInfor.qualityStaff}}</span></div>
								<div>检测类型：<span>{{productInfor.reportTypeName}}</span></div>
							</div>
						</div>
						<div @click='videoImgClick([productInfor.photo],0)' v-if='productInfor.photo!=null&&productInfor.photo' class="h_i_content_img">
							<img :src="img_url+productInfor.photo" alt="" />
						</div>
					</div>
				</div>

			</div>

			<!--原材采购 v-if='materialBuyList.length>0'-->
			<div class="yccg" v-if='false'>
				<div class="box_header">
					<div class="box_header_name">原材采购</div>
				</div>
				<div class="yccg_content">
					<div class="swiper-container yccg_swiper">
						<div class="swiper-wrapper">
							<div class="swiper-slide" v-for='(item,index) in materialBuyList'>
								<div class="yccg_hader">
									<span>{{item.operationTimeStr}}</span>
									<p>采购日期</p>
								</div>
								<img v-if='item.details[0].photo&&item.details[0].photo!=null' @click='videoImgClick([item.details[0].photo],0)' :src="$root.config.img_url+item.details[0].photo" alt="" />
								<div class="yccg_bottom">
									<p>品种：{{item.details[0].breedName}}</p>
									<p>规格：{{item.details[0].fullName}}</p>
									<p>等级：{{item.details[0].gradeName}}</p>
									<p>是否合格：{{item.details[0].qualifiedName}}</p>
									<p>生产时间：{{item.details[0].production}}</p>
									<p>采购时间：{{item.details[0].expirationDate}}</p>
									<p>客户名称：{{item.customerName}}</p>
								</div>
							</div>
						</div>
						<!-- 如果需要分页器 -->
						<div class="swiper-pagination yccg_pagination"></div>

						<!-- 如果需要导航按钮 -->
						<div class="swiper-button-prev"></div>
						<div class="swiper-button-next"></div>
					</div>
				</div>
			</div>

			<!--枸杞-->

			<!--生产过程；种植过程；养殖过程-->
			<div v-if='cultureEntityViewShow||processEntityListShow' class="production_process">
				<div class="box_header">
					<div class="box_header_name">种植档案</div>
				</div>

				<div v-if='cultureEntityViewShow' class="record_time">
					<div class="title">种植时间</div>
					<div class="time">
						{{cultureEntityView.startDate}}&nbsp;至&nbsp;{{cultureEntityView.endEnd}}
					</div>
				</div>

				<div v-if='processEntityListShow' class="farm">
					<div v-for='(item,index) in lookAndClose?processEntityList.slice(0,3):processEntityList' :key='item.processId' class="farm_list">
						<!--@click='videoImgClick(["../../dist/static/img/gq_logo.8640f00.png"],0,morenImg)'-->
						<div class="swiper_list swiper_list_img_c" v-if='(!item.photo||item.photo==null)&&(!item.video||item.video==bull)'>
							<img class="swiper_list_img" src="../assets/img/gq_logo.png" alt="" />
						</div>
						<div v-else class="swiper_list">
							<template v-if='item.photo&&item.photo!=null'>
								<img @click='videoImgClick(item.photo.split(","),0,item.video)' :src='img_url+item.photo.split(",")[0]' alt="" />
							</template>
							<video v-else-if='item.video&&item.video!=null' id="videoFarm" poster="../assets/img/video.png" controls="controls">
								<source :src="item.video" type="video/mp4" />
								<source id="ogg_src" :src="item.video" type="video/ogg"> 您的浏览器不支持 HTML5 video 标签。
							</video>
							<template v-if='item.photo&&item.photo!=null||item.video&&item.video!=null'>
								<div class="num">共{{item.photo&&item.photo!=null?item.photo.split(",").length:'0'}}张图{{item.video&&item.video!=null?'1个视频':''}}</div>
							</template>
						</div>
						<ul>
							<li>[{{index+1
								<10? '0'+(index+1):index+1}}]&nbsp;<span>{{item.processContent}}</span>
							</li>
							<li>操作时间：{{item.operatorDate}}</li>
							<li>操作人：{{item.chargeName}}</li>
							<li v-if='item.processMaterielViewList.length>0&&item.processMaterielViewList!=null'>投放品：<span v-for="(i,n) in item.processMaterielViewList">{{i.breedName}}<span v-if='n+1!=item.processMaterielViewList.length'>、</span></span>
							</li>
						</ul>
					</div>
					<!--查看更多-->
					<div v-if='processEntityList.length>3' class="look_more" @click='lookAndClose?lookAndClose=false:lookAndClose=true'>{{lookAndClose?'查看更多':'收起'}}</div>

				</div>
			</div>
			<!--收获信息-->
			<div v-if='recoveryEntityViewShow||materialBuyList.length>0' class="harvest_info">
				<div class="box_header">
					<div class="box_header_name">收获信息</div>
				</div>

				<div class="swiper-container harvest_info_swiper">
					<div class="swiper-wrapper">
						<div class="swiper-slide" v-for='(item,index) in recoveryEntityView'>
							<div class="title">
								<span>采收日期</span>
								<p>{{item.harvestDate}}</p>
							</div>
							<div class="bottom">
								<ul>
									<li>{{item.breedName}}</li>
									<li>规格：{{item.specifications}}</li>
									<li>等级：{{item.gradeName}}</li>
									<li>负责人：{{item.operatorStaffName}}</li>
									<!--<li>收获时间：{{item.harvestDate}}</li>-->
								</ul>
								<div class="jcjg">
									<div class="box_com_c_bottom_right">
										<div>检测结果：{{item.result}}</div>
										<div>检测标准：{{item.inspectionStandardName}}</div>
										<div>检测人：{{item.qualityStaff}}</div>
										<div>检测类型：{{item.reportTypeName}}</div>
									</div>
									<template v-if='item.photo&&item.photo!=null'>
										<img @click='videoImgClick([item.photo],0)' :src="img_url+item.photo" alt="" />
									</template>
								</div>
							</div>
						</div>
						<div class="swiper-slide" v-for='(item,index) in materialBuyList'>
							<div class="title">
								<span>采购日期</span>
								<p>{{item.operationTimeStr}}</p>
							</div>
							<div class="bottom">
								<ul>
									<li>{{item.details[0].breedName}}</li>
									<li>规格：{{item.details[0].fullName}}</li>
									<li>等级：{{item.details[0].gradeName}}</li>
									<p>生产时间：{{item.details[0].production}}</p>
						        	<!--<p>采购时间：{{item.details[0].expirationDate}}</p>-->
									<!--<li>客户名称：{{item.customerName}}</li>-->
									<!--<li>收获时间：{{item.harvestDate}}</li>-->
									
								</ul>
								<div class="jcjg jcjg_c">
									<div class="box_com_c_bottom_right ">
										<p>检测结果</p>
										<div class="jcjg_c_c">{{item.details[0].qualifiedName}}</div>
									</div>
									<img v-if='item.details[0].photo&&item.details[0].photo!=null' 
						        		@click='videoImgClick([item.details[0].photo],0)' 
						        		:src="$root.config.img_url+item.details[0].photo" alt="" 
						        	/>
								</div>
							</div>
						</div>
					</div>
					<!-- 如果需要分页器 -->
					<div class="swiper-pagination harvest_info_pagination"></div>
					<!-- 如果需要导航按钮 -->
					<div class="swiper-button-prev"></div>
					<div class="swiper-button-next"></div>
				</div>
			</div>

			<!--加工信息-->
			<div v-if='productInforShow' class=" process_info">
				<div class="box_header">
					<div class="box_header_name">加工信息</div>
				</div>
				<div class="top">
					<ul>
						<li>{{productInfor.breedName}}</li>
						<li>规格：<span>{{productInfor.specifications}}</span></li>
						<li>等级：<span>{{productInfor.gradeName}}</span></li>
						<li>负责人：<span>{{productInfor.operatorStaffName}}</span></li>
						<li>生产时间：<span>{{productInfor.operationTime}}</span></li>
					</ul>
				</div>
				<!--加工信息的生产过程-->
				<div class="process" v-if='productInforListProcessShow'>

					<div v-for='(item,index) in processLookAndClose?productInforListProcess.slice(0,3):productInforListProcess' :key='item.processId' class="farm_list">
						<!--@click='videoImgClick(["../../dist/static/img/gq_logo.8640f00.png"],0,morenImg)'-->
						<div class="swiper_list swiper_list_img_c" v-if='(!item.photo||item.photo==null)&&(!item.video||item.video==bull)'>
							<img class="swiper_list_img" src="../assets/img/gq_logo.png" alt="" />
						</div>
						<div v-else class="swiper_list">
							<template v-if='item.photo&&item.photo!=null'>
								<img @click='videoImgClick(item.photo.split(","),0,item.video)' :src='img_url+item.photo.split(",")[0]' alt="" />
							</template>
							<video v-else-if='item.video&&item.video!=null' id="videoFarm" poster="../assets/img/video.png" controls="controls">
								<source :src="item.video" type="video/mp4" />
								<source id="ogg_src" :src="item.video" type="video/ogg"> 您的浏览器不支持 HTML5 video 标签。
							</video>
							<template v-if='item.photo&&item.photo!=null||item.video&&item.video!=null'>
								<div class="num">共{{item.photo&&item.photo!=null?item.photo.split(",").length:'0'}}张图{{item.video&&item.video!=null?'1个视频':''}}</div>
							</template>
						</div>

						<ul>
							<li>[{{index+1<10? '0'+(index+1):index+1}}]&nbsp;<span>{{item.name}}</span>
							</li>
							<template v-if='item.operationDateStr'>
								<li>{{item.operationDateStr.split(' ')[0]}}</li>
							</template>
							<li v-else>时间未上传</li>
							<li>负责人：{{item.operatorStaffName}}</li>
							<li class="dian">工作参数：<span>{{item.processParameters}}</span></li>
							<li class="dian">作业环境：<span>{{item.operatingEnvironment}}</span></li>
							<li class="dian" v-if='item.outdetal.length>0&&item.outdetal!=null'>原料：<span v-for="(i,n) in item.outdetal">{{i.breedName}}<span v-if='n+1!=item.outdetal.length'>、</span></span>
							</li>
						</ul>
					</div>
					<!--查看更多-->
					<div v-if='productInforListProcess.length>3' class="look_more" @click='processLookAndClose?processLookAndClose=false:processLookAndClose=true'>{{processLookAndClose?'查看更多':'收起'}}</div>

				</div>
				<!--<div class="look_video" @click='goMonitoring(1)'>查看实时监控</div>-->
			</div>

			<div class="box_5" v-if='isTransShow'>
				<div class="box_header">
					<div class="box_header_name">流通信息</div>
				</div>
				<!--{{trans.origin}}-->
				<!--{{trans.destination}}-->
				<div class="box_5_content">
					<div class="box_5_top">
						<div class="title">出发地</div>
						<span>{{trans.origin}}</span>
					</div>
					<div class="box_5_bottom">
						<div class="title">
							<div class="title_c">目的地</div>
						</div>
						<span>{{trans.destination}}</span>
					</div>
					<ul class="bottom">
						<li>
							<p>车牌号</p>
							<p class="p">{{vehicleEntity.licenseNumber}}</p>
						</li>
						<li>
							<p>途中温度</p>
							<p class="p">{{trans.temperature}}℃</p>
						</li>
						<li>
							<p>负责人</p>
							<p class="p">{{trans.dutyStaffName}}</p>
						</li>
						<li>
							<p>运输时间</p>
							<p class="p">{{trans.startTime}}</p>
						</li>
					</ul>
				</div>
			</div>

			<!--广玉的面食分享-->
			<div v-if='isCookedShow' class="cooked">
				<div class="box_header">
					<div class="box_header_name">产品分享</div>
				</div>
				<div class="swiper-container cookedSwiper">
					<div class="swiper-wrapper">
						<div v-if='cookbooks[0].video!=null&&cookbooks[0].video' class="swiper-slide">
							<!--autoplay="autoplay" controls="controls"-->
							<!--poster="../assets/img/banner.png"-->
							<video id="videoCooked" poster="../assets/img/video.png" controls="controls">
								<source :src="video_url+cookbooks[0].video" type="video/mp4" />
								<source id="ogg_src" :src="video_url+cookbooks[0].video" type="video/ogg"> 您的浏览器不支持 HTML5 video 标签。
							</video>
						</div>
						<div @click='videoImgClick(cookbooks[0].photo.split(","),index)' v-if='cookbooks[0].photo!=null&&cookbooks[0].photo' v-for='(itemPhoto,index) in cookbooks[0].photo.split(",")' class="swiper-slide">
							<img :data-src="img_url+itemPhoto" alt="" class="swiper-lazy" />
						</div>
					</div>
					<div class="swiper-pagination-cooked"></div>
				</div>
				<div v-if='cookbooks[0].describe!=null' class="text">
					{{cookbooks[0].describe}}
				</div>
			</div>
		</div>

		<!--弹出查看图片-->
		<van-popup v-model="showImg">
			<div class="swiper-container showImg" id='showImg'>
				<div class="swiper-wrapper">

				</div>
				<!-- 如果需要分页器 -->
				<div class="swiper-pagination-showImg"></div>
			</div>
			<div style="color: white;margin-top: .3rem;text-align: center;" @click='showImg=false'>关闭</div>
		</van-popup>
		<div class="footer" style="background: #FFFFFF;">
			<p class='footerName' style="color: #585858;padding: 0 0 0.42rem 0;">宁夏杞彩回乡土特产开发有限公司</p>
			<!--<p class="jszc" style="color: white;">技术支持：成都九洲电子信息系统股份有限公司</p>-->
		</div>
		<div :style="scrollTopStyle" @click="goTop" class="scrollTop">
			<van-icon name="upgrade" />
		</div>
	</div>
</template>

<script>
	import Vue from 'vue'
	//	import "./../scss/index.scss"
	import gb_logo from '../assets/img/icon_laba.png'
	import logo from '../assets/img/gq_logo.png'
	import videoImg from '../assets/img/video.png'
	import guangyu from '../assets/img/index_guangyu.png'
	import hefeng from '../assets/img/index_hefeng.png'
	import jinhe from '../assets/img/index_jinhe.png'
	import ningbao from '../assets/img/index_ningbao.png'
	import wanjia from '../assets/img/index_wanjia.png'
	import yijude from '../assets/img/index_yijude.png'
	import yufeng from '../assets/img/index_yufeng.png'
	import yuxin from '../assets/img/index_yuxin.png'
	import bannerOld from '../assets/img/img_img.png'
	import mrVideoImg from '../assets/img/video.png'
	import morenImg from '../assets/img/gq_logo.png'
	import { Toast, Popup, Button, NoticeBar ,Icon} from 'vant';
	var componentsArr = [Toast, Popup, Button, NoticeBar,Icon]
	Vue.use(function(Vue) {
		componentsArr.map(component => {
			Vue.component(component.name, component);
		});
		for(var key in componentsArr[0].components) {
			Vue.component(componentsArr[0].components[key].name, componentsArr[0].components[key]);
		}
	})
	export default {
		data() {
			return {
				scrollTopStyle: {
					display: "none"
				},
				morenImg: morenImg,
				lookAndClose: true,
				processLookAndClose: true,
				title: TITLE,
				count: 0,
				farmEntityView: false,
				isContentShow: false,
				company: {
					name: ''
				},
				//产品信息
				varietyEntityView: {
					breedName: '',
					expirationDate: '',
					gradeName: '',
					organicCropsName: '',
					production: '',
					specifications: '',
					name: '',
					photo: '',
				},
				//农事过程
				processEntityList: [],
				processEntityListShow: false,
				//收获信息
				//				recoveryEntityView: {
				//					breedName: '',
				//					gradeName: '',
				//					harvestDate: '',
				//					inspectionStandardName: '',
				//					operatorStaffName: '',
				//					qualityStaff: '',
				//					photo: null,
				//					reportTypeName: '',
				//					result: '',
				//				},
				recoveryEntityView: [],
				recoveryEntityViewShow: true,
				//加工信息
				productInfor: {
					breedName: '',
					gradeName: '',
					inspectionStandardName: '',
					operationTime: '',
					operatorStaffName: '',
					qualityStaff: '',
					reportTypeName: '',
					specifications: '',
					result: '',
					photo: null,
				},
				//加工信息的生产过程
				productInforListProcess: [],
				productInforListProcessShow: true,
				productInforShow: true,
				videoShowSwiperArr: [],
				appendArr: [],
				processName: '种植过程',
				messageName: '采收信息',
				record: '种植档案',
				img_url: IMG_URL,
				video_url: VIDEO_URL,
				cultureEntityView: {
					startDate: '',
					endEnd: '',
				},
				cultureEntityViewShow: false,
				bannerStyle: {},
				isJinheImgShow: false,
				isCookedShow: false,
				cookbooks: [],
				imgStyle: {},
				gb_logo: gb_logo,
				isNoticeShow: false,
				newsEnityList: [{
					title: '',
				}],
				qiyeStyle: {},
				cookbooksName: '',
				farmSwiperChildrenObj: {},
				farmCount: 0,
				showImg: false,
				showImgObj: {},
				trans: {},
				vehicleEntity: {},
				isTransShow: false,
				materialBuyList: [],
			}
		},
		computed: {

		},
		mounted() {
			//title判断
			this.titleJudge();
			this.init();
			this.imgSwiper();
			this.scroll()
		},

		methods: {
			
			goMonitoring() {
				this.$router.push({
					name: 'monitoring',
					query: {
						str: num,
						traceablityNo: this.$route.query.traceablityNo
					}
				})
			},
			yccgSwiper(class1, page1) {
				var swiper = new Swiper(class1, {
					observeParents: true, //修改swiper的父元素时，自动初始化swiper
					//						freeMode: true,
					spaceBetween: 20,
					lazyLoading: true,
					lazyLoadingOnTransitionStart: true,
					lazyLoadingInPrevNext: true,
					lazyLoadingInPrevNextAmount: 1,
					autoplay: 4000,
					pagination: page1,
					paginationType: 'fraction',
					// 如果需要前进后退按钮
					nextButton: '.swiper-button-next',
					prevButton: '.swiper-button-prev',
				});
			},
			box2swiper(class_1, class_2, paginationName_2, paginationName_1) {
				this.$nextTick(() => {
					var vm = this
					//'.box_2_id'
					var swiper = new Swiper(class_1, {
						observeParents: true, //修改swiper的父元素时，自动初始化swiper
						slidesPerView: class_2 ? 1.28 : 1,
						centeredSlides: true,
						spaceBetween: 20,
						lazyLoading: true,
						lazyLoadingOnTransitionStart: true,
						lazyLoadingInPrevNext: true,
						lazyLoadingInPrevNextAmount: 1,
						autoplay: 4000,
						freeMode: class_2 ? true : false,
						pagination: paginationName_1,
						paginationType: 'fraction',
					});
					//'.box_2_children'
					if(class_2) {
						var farmSwiperChildren = new Swiper(class_2, {
							observeParents: true,
							lazyLoading: true,
							lazyLoadingOnTransitionStart: true,
							lazyLoadingInPrevNext: true,
							lazyLoadingInPrevNextAmount: 1,
							centeredSlides: true,
							//.swiper-pagination-box-4
							pagination: paginationName_2,
							paginationType: 'fraction',
						});
					}

				})

			},
			videoImgClick(imgArr, index, video) {
				var vm = this;
				var arr = [];
				var count = null;
				this.showImg = true;
				//判断传下来的东西来进行相应的swiper
				this.showImgObj.removeAllSlides();
				arr = this.videoImgStr(imgArr, video)
				vm.showImgObj.appendSlide(arr);
				count = arr.length
				vm.showImgObj.slideTo(1);
				vm.showImgObj.slideTo(index);
				for(var i = 0; i < vm.showImgObj.slides.length; i++) {
					vm.showImgObj.slides[i].style.transform = 'translate3d(' + ((count - 1) * 0.5) * vm.showImgObj.width + 'px, 0px, 0px)';
				}
			},
			videoImgStr(imgArr, video) {
				var arr = [];
				for(let m = 0; m < imgArr.length; m++) {
					var str = `<div  class="swiper-slide">
						<img src=${IMG_URL}${imgArr[m]} alt="" class="swiper-lazy"/>
						</div>`
					arr.push(str)
				}
				if(video) {
					arr.push(`
						<div  class="swiper-slide">
						<video id="videoFarm" poster=${mrVideoImg} controls="controls">
								<source src=${video} type="video/mp4" />
								<source id="ogg_src" src=${video} type="video/ogg"> 您的浏览器不支持 HTML5 video 标签。
						</video>
						</div>
						`)
				}
				return arr
			},
			imgSwiper() {
				var vm = this
				var showImgObj = new Swiper('.showImg', {
					observeParents: true,
					lazyLoading: true,
					lazyLoadingOnTransitionStart: true,
					width: window.innerWidth, //innerHeight
					height: window.innerHeight,
					centeredSlides: true,
					lazyLoadingInPrevNext: true,
					lazyLoadingInPrevNextAmount: 1,
					pagination: '.swiper-pagination-showImg',
					paginationType: 'fraction',
					onSlideChangeStart: function(swiper) {
						Object.assign(vm.showImgObj, swiper)
					},
					onClick: function() {
						//vm.showImg = false;
					}
				});
				Object.assign(this.showImgObj, showImgObj)
			},
			init() {
				this.$root.ajax({
					//url: this.$root.config.api_url + "traceablity/getTraceablityInfo",
					name: 'traceablity/getTraceablityInfo',
					type: 'get',
					param: {
						traceablityNo: this.$route.query.traceablityNo,
					},
				}).then((d) => {
					if(d.status == 'success' && typeof d.result != 'string') {
						this.isContentShow = true;
						//判断基地信息
						if(d.result.farmEntityViewList == null || d.result.farmEntityViewList.length <= 0) {
							this.farmEntityView = false;
							this.qiyeStyle = {
								width: '7rem'
							}
						} else {
							this.qiyeStyle = {
								width: '3.3rem'
							}
							this.farmEntityView = true;
						}
						//判断实时动态
						if(d.result.newsEnityList != null && d.result.newsEnityList.length > 0) {
							this.isNoticeShow = true;
							this.newsEnityList = d.result.newsEnityList
						}

						this.box2swiper('.box_2_id', '.box_2_children', '.swiper-pagination-box-2')
						//产品信息
						Object.assign(this.company, d.result.company)
						Object.assign(this.varietyEntityView, d.result.varietyEntityView)
						//原材采购
						

						if(this.varietyEntityView.photo != null) {
							this.varietyEntityView.photo = IMG_URL + this.varietyEntityView.photo;
						} else {
							this.varietyEntityView.photo = logo;
							this.imgStyle = {
								width: '1.42rem',
								height: '1.42rem'
							}
						}
						
						if(d.result.cultureProcessView.cultureEntityViewList.length == 0) {
							this.recoveryEntityViewShow = false; //收获信息
							this.cultureEntityViewShow = false; //档案判断
							this.processEntityListShow = false; //农事过程
							if(d.result.materialBuyList != null && d.result.materialBuyList) {
								this.recoveryEntityViewShow = true;
								this.materialBuyList = d.result.materialBuyList
								this.$nextTick(() => {
									this.yccgSwiper('.harvest_info_swiper', '.harvest_info_pagination');
								})
							}
						} else {
							
							if(
								d.result.cultureProcessView.cultureEntityViewList[0].cultureId &&
								d.result.cultureProcessView.cultureEntityViewList[0].cultureId != null
							) {
								this.recoveryEntityViewShow = true;
								this.cultureEntityViewShow = true;
								this.processEntityListShow = true;
								if(d.result.materialBuyList != null && d.result.materialBuyList) {
									this.materialBuyList = d.result.materialBuyList
								}
								
								//recoveryEntityViewList收获信息   !!!!!!!!这里是一个数组，后期修改
//								Object.assign(this.recoveryEntityView, d.result.cultureProcessView.recoveryEntityViewList)
								this.recoveryEntityView=d.result.cultureProcessView.recoveryEntityViewList
								console.log(this.recoveryEntityView)
								
								this.$nextTick(() => {
									this.yccgSwiper('.harvest_info_swiper', '.harvest_info_pagination');
								})

								//档案时间
								Object.assign(this.cultureEntityView, d.result.cultureProcessView.cultureEntityViewList[0])
								this.cultureEntityViewShow = true;
								//农事过程 processEntityList
								if(d.result.cultureProcessView.processEntityList == null || d.result.cultureProcessView.processEntityList.length <= 0) {
									this.processEntityListShow = false; //农事过程
								} else {
									this.processEntityList = d.result.cultureProcessView.processEntityList
									//添加video URL
									this.forPopupArr(this.processEntityList, 'farmChildrenArr')
									this.$nextTick(() => {
										this.farmSwiper();
										this.farmSwiperChildren();
									})
								}
							} else {
								this.recoveryEntityViewShow = false; //收获信息
								this.cultureEntityViewShow = false; //档案判断
								this.processEntityListShow = false; //农事过程
								if(d.result.materialBuyList != null && d.result.materialBuyList) {
									this.recoveryEntityViewShow = true;
									this.materialBuyList = d.result.materialBuyList
									this.$nextTick(() => {
										this.yccgSwiper('.harvest_info_swiper', '.harvest_info_pagination');
									})
								}
							}

						}
						//加工信息

						if(d.result.productInfor == null) {
							this.productInforShow = false;
						} else {
							Object.assign(this.productInfor, d.result.productInfor)
							if(this.productInfor.photo != null) {
								this.productInfor.photo = this.productInfor.photo
							}
							this.productInforListProcess = d.result.productInfor.listProcess
							if(this.productInforListProcess.length > 0) {
								this.forPopupArr(this.productInforListProcess, 'videoShowSwiperArr')
								this.box2swiper('.box_4_id', '.box_4_children', '.swiper-pagination-box-4', '.swiper-pagination-box-f-4')
								this.$nextTick(() => {
									this.processSwiper();
									this.videoSwiper();
								})
							} else {
								this.productInforListProcessShow = false;
							}
						}
						//产品流向
						if(d.result.productInfor != null && d.result.trans) {
							this.isTransShow = true
							this.trans = d.result.trans
							this.vehicleEntity = d.result.vehicleEntity
						}
						//面食分享
						if(d.result.cookbooks != null && d.result.cookbooks.length > 0) {
							this.isCookedShow = true;
							this.cookbooks = d.result.cookbooks
							this.$nextTick(() => {
								this.cookedSwiper();
							})
						}
					} else {
						this.isContentShow = false; //false
						Toast(d.result);
					}
				})
			},

			//农事过程的Swiper
			farmSwiper() {
				var vm = this;
				var farmSwiper = new Swiper('.farmSwiper', {
					observeParents: true, //修改swiper的父元素时，自动初始化swiper
					paginationClickable: true,
					autoplay: 4000,
					lazyLoading: true,
					lazyLoadingOnTransitionStart: true,
					lazyLoadingInPrevNext: true,
					lazyLoadingInPrevNextAmount: 1,
					centeredSlides: true,
					effect: 'coverflow',
					slidesPerView: 2, //显示几个silder auto
					spaceBetween: -180, //间隔
					slidesOffsetBefore: 0, //左侧的偏移量
					coverflow: {
						rotate: 0, //两侧的翻转角度
						stretch: 0, //左右的间隔
						depth: 300, //后面的缩小好多
						modifier: 2,
						slideShadows: true
					},
					pagination: '.swiper-pagination-farm',
					paginationType: 'fraction',
					onInit: function(swiper) {
						vm.farmCount = swiper.activeIndex;
					},
					//这里协商onClick是因为有时候滑动的时候onSlideChangeStart或者onSlideChangeEnd不执行调用的
					onSlideChangeStart: function(swiper) {
						vm.farmCount = swiper.activeIndex;
					},
					onSlideChangeEnd: function(swiper) {
						vm.farmCount = swiper.activeIndex;
					},
					onClick: function(swiper, e) {
						vm.farmCount = swiper.activeIndex;
					}
				});
			},
			//视频和图片
			farmSwiperChildren() {
				var vm = this
				var farmSwiperChildren = new Swiper('.farmSwiperChildren', {
					observeParents: true,
					lazyLoading: true,
					lazyLoadingOnTransitionStart: true,
					lazyLoadingInPrevNext: true,
					lazyLoadingInPrevNextAmount: 1,
					centeredSlides: true,
				});
			},
			//视频和图的Swiper
			videoSwiper() {
				var vm = this;
				var videoSwiper = new Swiper('.videoSwiper', {
					//					autoplay: 5000,
					pagination: '.swiper-pagination-video',
				});
				//tap这种300ms的延迟点透在Safari上面有问题。是否是延迟加载造成的还未测试！
				//				if(!Array.isArray(videoSwiper)) {
				//					videoSwiper = [videoSwiper]
				//				}
				//				for(let m = 0; m < videoSwiper.length; m++) {
				//					if(vm.$root.getOS() == 'pcOS') {
				//						videoSwiper[m].on('click', function(swiper) {
				//							vm.videoSwiperOnclick(swiper)
				//						})
				//					} else {
				//						//QQ打开，会有点透事件，占时无法解决
				//						videoSwiper[m].on('tap', function(swiper) {
				//							vm.videoSwiperOnclick(swiper)
				//						})
				//					}
				//				}
			},
			//加工信息里面加工过程的swiper
			processSwiper() {
				var vm = this;
				var process = new Swiper('#process', {
					autoplay: 4000,
					observer: true,
					observeParents: true, //修改swiper的父元素时，自动初始化swiper
					paginationClickable: true,
					lazyLoading: true,
					lazyLoadingOnTransitionStart: true,
					lazyLoadingInPrevNext: true,
					lazyLoadingInPrevNextAmount: 1,
					centeredSlides: true, //不居中的话那个swiper.activeIndex有问题
					slidesPerView: 'auto', //显示几个silder auto
					spaceBetween: 15, //间隔
					slidesOffsetBefore: 0, //左侧的偏移量
					pagination: '.swiper-pagination-farm',
					paginationType: 'fraction',
					//					slidesPerView: 1.28,
					//					freeMode: true,
					initialSlide: 0,
					onInit: function(swiper) {
						//这里最主要是获取最外层的swiper点击的是第几个
						vm.videoCount = swiper.activeIndex;
					},
					onSlideChangeStart: function(swiper) {
						vm.videoCount = swiper.activeIndex;
					}
				});
			},
			//面食分享的swiper
			cookedSwiper() {
				var vm = this
				var cookedSwiper = new Swiper('.cookedSwiper', {
					observeParents: true,
					lazyLoading: true,
					centeredSlides: true,
					lazyLoadingOnTransitionStart: true,
					lazyLoadingInPrevNext: true,
					lazyLoadingInPrevNextAmount: 1,
					autoplay: 4000,
					pagination: '.swiper-pagination-cooked',
					onSlideChangeStart: function(swiper) {

					},
					paginationType: 'fraction',
					onInit: function(swiper) {

					},
					onClick: function(swiper) {

					}
				});
			},
			noticeClick() {
				this.$router.push({ name: 'news', query: { newsId: this.newsEnityList[0].newsId, traceablityNo: this.$route.query.traceablityNo } })
			},
			//循环处理自动弹出的swiper
			forPopupArr(arr, str) {
				for(let m = 0; m < arr.length; m++) {
					this[str + m] = []
					if(arr[m].video) {
						if(arr[m].video.indexOf("://") > -1) {
							arr[m].video = arr[m].video
						} else { //在线视频
							arr[m].video = VIDEO_URL + arr[m].video
						}
						this[str + m].push(arr[m].video)
					}
					if(arr[m].photo) {
						var arrImg = arr[m].photo.split(',');
						for(let a = 0; a < arrImg.length; a++) {
							this[str + m].push(IMG_URL + arrImg[a])
						}
					}
				}
			},
			titleJudge() {
				if(TITLE == '宁夏伊聚徳农工贸有限公司' || TITLE == '宁夏金河科技股份有限公司') {
					this.processName = '养殖过程'
					this.messageName = '收获信息'
					this.record = '养殖档案'
				}
				if(TITLE == '宁夏金河科技股份有限公司') {
					this.cookbooksName = '金河产品分享'
					this.isJinheImgShow = true;
					this.messageName = '收奶信息'
					this.bannerStyle = {
						backgroundImage: `url(${jinhe})`,
						backgroundSize: 'contain'
					}
				} else if(TITLE == '灵武市玉锋粮油综合加工有限公司') {
					this.bannerStyle = {
						backgroundImage: `url(${yufeng})`,
						backgroundSize: 'contain'
					}
				} else if(TITLE == '宁夏广玉面粉有限公司') {
					this.cookbooksName = '广玉面食分享'
					this.bannerStyle = {
						backgroundImage: `url(${guangyu})`,
						backgroundSize: 'contain'
					}
				} else if(TITLE == '宁夏贺丰种业有限公司') {
					this.bannerStyle = {
						backgroundImage: `url(${hefeng})`,
						backgroundSize: 'contain'
					}
				} else if(TITLE == '宁夏宁宝世家食品科技有限公司') {
					this.bannerStyle = {
						backgroundImage: `url(${ningbao})`,
						backgroundSize: 'contain'
					}
				} else if(TITLE == '宁夏万家香食品有限公司') {
					this.bannerStyle = {
						backgroundImage: `url(${wanjia})`,
						backgroundSize: 'contain'
					}
				} else if(TITLE == '宁夏伊聚徳农工贸有限公司') {
					this.bannerStyle = {
						backgroundImage: `url(${yijude})`,
						backgroundRepeat: 'no-repeat',
						backgroundSize: 'contain'
					}
				} else if(TITLE == '银川育新枸杞种业有限公司') {
					this.bannerStyle = {
						backgroundImage: `url(${yuxin})`,
						backgroundSize: 'contain'
					}
				} else {
					this.bannerStyle = {
						backgroundImage: `url(${bannerOld})`,
						backgroundSize: 'contain'
					}
				}
			},
			scroll(){
				var vm = this;
				var bannerHeight = 280
				document.onscroll = function() {
					var scrollTop = 0;
					if(document.documentElement && document.documentElement.scrollTop) {
						scrollTop = document.documentElement.scrollTop;
					} else if(document.body) {
						scrollTop = document.body.scrollTop;
					}
	
					if(scrollTop > bannerHeight) {
						vm.orderDateStyle = {
							position: "fixed",
							top: 0
						}
						vm.isShowOrderDateFlexd = true;
					} else {
						vm.orderDateStyle = {
							position: "",
							top: 0
						}
						vm.isShowOrderDateFlexd = false;
					}
					if(scrollTop > 280) {
						vm.scrollTopStyle = { display: 'block' }
					} else {
						vm.scrollTopStyle = { display: 'none' }
					}
				}
			},
			goTop() {
				var timer = setInterval(function() {
					//获取滚动条的滚动高度
					var osTop = document.documentElement.scrollTop || document.body.scrollTop;
					//用于设置速度差，产生缓动的效果
					var speed = Math.floor(-osTop / 6);
					if(document.documentElement && document.documentElement.scrollTop) {
						document.documentElement.scrollTop = document.body.scrollTop = osTop + speed;
					} else if(document.body) {
						document.body.scrollTop = osTop + speed;
					}
					//      document.documentElement.scrollTop = document.body.scrollTop = osTop + speed;
					var isTop = true; //用于阻止滚动事件清除定时器
					if(osTop == 0) {
						clearInterval(timer);
					}
				}, 30);
			},
		}
	}
</script>

<style lang="scss">
	@import './../scss/index.scss';
</style>