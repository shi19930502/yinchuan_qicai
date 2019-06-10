<template>
	<div class="index">
		<!--:style='bannerStyle'-->
		<div class="header">
			<van-notice-bar v-if='isNoticeShow' @click='noticeClick' :text="newsEnityList[0].title" :left-icon="gb_logo" />
		</div>
		
		<div class="header_nav">
			<router-link :to="{name:'enterprise',query:{traceablityNo:$route.query.traceablityNo}}">
				<div class="header_nav_list">
					<p>企业信息</p>
				</div>
			</router-link>
			<router-link :to="{name:'client',query:{traceablityNo:$route.query.traceablityNo,varietyId:this.varietyEntityView.varietyId,varietySpecId:this.varietyEntityView.varietySpecId}}">
				<div class="header_nav_list">
					<p>评价反馈</p>
				</div>
			</router-link>
			<router-link :to="{name:'monitoring',query:{str:1,traceablityNo:$route.query.traceablityNo,varietyId:this.varietyEntityView.varietyId,varietySpecId:this.varietyEntityView.varietySpecId}}">
				<div class="header_nav_list">
					<p @click='goMonitoring(0)'>实时监控</p>
				</div>
			</router-link>
		</div>
		<div class="content" v-if='isContentShow'>

			<div class="product_info">
				<div class="box_header">
					<img src="../assets/img/con_bian.png" alt="" />
					<div class="box_header_name">产品信息</div>
				</div>
				<div class="p_i_content">
					<img :style='imgStyle' :src="varietyEntityView.photo" alt="" />
					<ul>
						<li>{{varietyEntityView.breedName}}</li>
						<li>{{company.name}}</li>
						<li>品种：<span>{{varietyEntityView.breedName}}</span></li>
						<li>规格：<span>{{varietyEntityView.specifications}}</span></li>
						<li>等级：<span>{{varietyEntityView.gradeName}}</span></li>
						<!--<li v-if='varietyEntityView.organicCropsName=="是"'>是否有机：<span>{{varietyEntityView.organicCropsName}}</span></li>-->
						<li>生产时间：<span>{{varietyEntityView.production}}</span></li>
						<li>过期时间：<span>{{varietyEntityView.expirationDate}}</span></li>
					</ul>
				</div>
				
				<div class="traceablity_no" v-if='$route.query.traceablityNo'>
					<div class="traceablity_no_name">追溯码</div>
					<div class="traceablity_no_content">{{$route.query.traceablityNo}}</div>
				</div>
				
				<div v-if='productInforShow' class="h_i_content">
					<div @click='videoImgClick([productInfor.photo],0)' v-if='productInfor.photo!=null&&productInfor.photo' class="h_i_content_img">
						<img :src="img_url+productInfor.photo" alt="" />
					</div>
					<div class="h_i_c_bottom">
						<div class="h_i_c_bottom_center">
							<div>检测结果：<span class="hege">{{productInfor.result}}</span></div>
							<div>检测标准：<span>{{productInfor.inspectionStandardName}}</span></div>
							<div>检测人：<span>{{productInfor.qualityStaff}}</span></div>
							<div>检测类型：<span>{{productInfor.reportTypeName}}</span></div>
						</div>
					</div>
				</div>
				
			</div>
			
			<!--原材采购-->
			<div class="yccg" v-if='materialBuyList.length>0'>
				<div class="box_header">
					<img src="../assets/img/con_bian.png" alt="" />
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
					        	<img v-if='item.details[0].photo&&item.details[0].photo!=null' 
					        		@click='videoImgClick([item.details[0].photo],0)' 
					        		:src="$root.config.img_url+item.details[0].photo" alt="" 
					        	/>
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

			<div v-if='false' class="box_2">
				<div class="box_header">
					<img src="../assets/img/con_bian.png" alt="" />
					<div class="box_header_name">种植档案</div>
				</div>
				<div class="box_2_content" v-if='cultureEntityViewShow'>
					<div class="box_2_header">
						种植时间：{{cultureEntityView.startDate}}&nbsp;&nbsp;至&nbsp;&nbsp;{{cultureEntityView.endEnd}}
					</div>
					<div class="dian_henxian">
						<!--<div class="dian">...</div>-->
						<div class="henxian"></div>
						<!--<div class="dian">...</div>-->
					</div>
					<div class="box_2_bottom" v-if='processEntityListShow'>
						<div id='box2id' class="swiper-container box_2_id">
							<div class="swiper-wrapper">
								<div v-for='(item, index) in processEntityList' class="swiper-slide box_2_swiper_1">
									<div class="box_swiper_header">
										<div class="time">
											{{item.operatorDate}}
										</div>
										<span>{{index+1}}</span>
									</div>
									<div id='box2idChildren' class="swiper-container   box_2_children">
										<div class="swiper-wrapper">
											<div v-if='item.video&&item!=null' class="swiper-slide">
												<video id="videoFarm" poster="../assets/img/video.png" controls="controls">
													<source :src="item.video" type="video/mp4" />
													<source id="ogg_src" :src="item.video" type="video/ogg"> 您的浏览器不支持 HTML5 video 标签。
												</video>
											</div>
											<template v-if='item.photo&&item.photo!=null'>
												<div @click='videoImgClick(item.photo.split(","),index)' v-for='(itemPhoto,index) in item.photo.split(",")' class="swiper-slide">
													<img :data-src="img_url+itemPhoto" alt="" class="swiper-lazy" />
												</div>
											</template>

											<div></div>
										</div>
									</div>
									<div class="swiper-pagination-box-2"></div>

									<div class="box_2_list">
										<div class="title">{{item.processContent}}</div>
										<p>操作时间：{{item.operatorDate}}</p>
										<p>操作人：{{item.chargeName}}</p>
										<p v-if='item.processMaterielViewList.length>0&&item.processMaterielViewList!=null'>
											投放品：<span v-for="(i,n) in item.processMaterielViewList">{{i.breedName}}<span v-if='n+1!=item.processMaterielViewList.length'>、</span></span>
										</p>
									</div>
								</div>
							</div>
							
						</div>
					</div>
					<div class="look_video" @click='goMonitoring(0)'>查看实时监控</div>
				</div>
			</div>
			
			<div v-if='false' class="box_3">
				<div class="box_header">
					<img src="../assets/img/con_bian.png" alt="" />
					<div class="box_header_name">收获信息</div>
				</div>
				<div class="box_3_content">
					<ul>
						<li>商品名称：{{recoveryEntityView.breedName}}</li>
						<li>规格：{{recoveryEntityView.specifications}}</li>
						<li>等级：{{recoveryEntityView.gradeName}}</li>
						<li>收获人：{{recoveryEntityView.operatorStaffName}}</li>
						<li>收获时间：{{recoveryEntityView.harvestDate}}</li>
					</ul>

					<div class="box_com_c_bottom">
						<template v-if='recoveryEntityView.photo&&this.recoveryEntityView.photo!=null'>
							<img @click='videoImgClick([recoveryEntityView.photo],0)' :src="img_url+recoveryEntityView.photo" alt="" />
						</template>
						<div class="box_com_c_bottom_right">
							<div>检测结果：{{recoveryEntityView.result}}</div>
							<div>检测标准：{{recoveryEntityView.inspectionStandardName}}</div>
							<div>检测人：{{recoveryEntityView.qualityStaff}}</div>
							<div>检测类型：{{recoveryEntityView.reportTypeName}}</div>
						</div>
					</div>

				</div>
			</div>

			<div v-if='false' class="box_4">
				<div class="box_header">
					<img src="../assets/img/con_bian.png" alt="" />
					<div class="box_header_name">加工信息</div>
				</div>

				<div class=" box_4_content" v-if='productInforShow'>
					<ul class="box_4_top">
						<li>{{productInfor.breedName}}</li>
						<li>规格：{{productInfor.specifications}}</li>
						<li>等级：{{productInfor.gradeName}}</li>
						<li>负责人：{{productInfor.operatorStaffName}}</li>
						<li>生产时间：{{productInfor.operationTime}}</li>
					</ul>
					<div v-if='productInforListProcessShow' class="box_2_bottom">
						<div class="swiper-container box_4_id">
							<div class="swiper-wrapper">
								<div v-for='(item, index) in productInforListProcess' class="swiper-slide box_2_swiper_1">
									<div id='box2idChildren' class="swiper-container   box_4_children">
										<div class="swiper-wrapper">
											<div v-if='item.video&&item!=null' class="swiper-slide">
												<video id="videoFarm" poster="../assets/img/video.png" controls="controls">
													<source :src="item.video" type="video/mp4" />
													<source id="ogg_src" :src="item.video" type="video/ogg"> 您的浏览器不支持 HTML5 video 标签。
												</video>
											</div>
											<template v-if='item.photo&&item.photo!=null'>
												<div @click='videoImgClick(item.photo.split(","),index)' v-for='(itemPhoto,index) in item.photo.split(",")' class="swiper-slide">
													<img :data-src="img_url+itemPhoto" alt="" class="swiper-lazy" />
												</div>
											</template>

										</div>
									</div>

									<div v-if='item.video&&item!=null||item.photo&&item.photo!=null' class="swiper-pagination-box-4"></div>

									<div class="box_2_list">
										<div class="title">{{item.name}}</div>
										<p>操作时间：{{item.operationDateStr}}</p>
										<p>负责人：{{item.operatorStaffName}}</p>
										<p>工作参数：{{item.processParameters}}</p>
										<p>作业环境：{{item.operatingEnvironment}}</p>
										<template v-if='item.outdetal.length>0&&item.outdetal!=null'>
											<p>
												原料：<span v-for="(i,n) in item.outdetal">{{i.breedName}}<span v-if='n+1!=item.outdetal.length'>、</span></span>
											</p>
										</template>

									</div>
								</div>
							</div>
							<div class="swiper-pagination-box-f-4"></div>
						</div>
					</div>
					<div class="look_video" @click='goMonitoring(1)'>查看实时监控</div>
				</div>
			</div>
			
			
			<!--枸杞-->
			
			
			<!--生产过程；种植过程；养殖过程-->
			<div v-if='cultureEntityViewShow||processEntityListShow' class="production_process">
				<div class="box_header">
					<img src="../assets/img/con_bian.png" alt="" />
					<div class="box_header_name">种植档案</div>
				</div>
				<div v-if='cultureEntityViewShow' class="record_time">
					<div class="record_time_c">
						<div class="record_time_c_n">开始时间</div>
						<div class="record_time_c_c">{{cultureEntityView.startDate}}</div>
					</div>
					<div class="record_hx"></div>
					<div class="record_start_c">
						<div class="record_time_c_n">结束时间</div>
						<div class="record_time_c_c">{{cultureEntityView.endEnd}}</div>
					</div>
				</div>
				<div v-if='processEntityListShow' class="farm">
					<div class="swiper-container farmSwiper" id='farm'>
						<div class="swiper-wrapper">
							<div v-for='(item, index) in processEntityList' class="swiper-slide swiper-slideC ">
								<div class="content_header">{{processName}}</div>
								<div class="biaoqian">{{index+1}}</div>
								<ul>
									<li>{{item.processContent}}</li>
									<li>{{item.operatorDate}}</li>
									<li class="swiperChildre" style="margin-bottom: .12rem;">
										<div class="swiper-container farmSwiperChildren">
											<div class="swiper-wrapper ">
												<div v-if='item.video&&item!=null' class="swiper-slide">
													<video id="videoFarm" poster="../assets/img/video.png" controls="controls">
														<source :src="item.video" type="video/mp4" />
														<source id="ogg_src" :src="item.video" type="video/ogg"> 您的浏览器不支持 HTML5 video 标签。
													</video>
												</div>
												<div @click='videoImgClick(item.photo.split(","),index)' v-if='item.photo' v-for='(itemPhoto,index) in item.photo.split(",")' class="swiper-slide">
													<img :data-src="img_url+itemPhoto" alt="" class="swiper-lazy" />
												</div>
											</div>
										</div>
									</li>
									<li class="dian"><i></i>操作人：<span>{{item.chargeName}}</span></li>
									<li class="dian" v-if='item.processMaterielViewList.length>0&&item.processMaterielViewList!=null'><i></i>投放品：<span v-for="(i,n) in item.processMaterielViewList">{{i.breedName}}<span v-if='n+1!=item.processMaterielViewList.length'>、</span></span>
									</li>
								</ul>
							</div>
						</div>
						<!-- 如果需要分页器 -->
						<div class="swiper-pagination-farm"></div>
					</div>
				</div>
			</div>
			<!--收获信息-->
			<div v-if='recoveryEntityViewShow' class="harvest_info">
				<div class="box_header">
					<img src="../assets/img/con_bian.png" alt="" />
					<div class="box_header_name">收获信息</div>
				</div>
				<ul>
					<li>{{recoveryEntityView.breedName}}</li>
					<!--<li>规格：<span>{{recoveryEntityView.specifications}}</span></li>-->
					<li v-if='isJinheImgShow'><img src="../assets/img/sh_jinhe.png" alt="" /></li>
					<li>等级：<span>{{recoveryEntityView.gradeName}}</span></li>
					<li>收获人：<span>{{recoveryEntityView.operatorStaffName}}</span></li>
					<li>收获时间：<span>{{recoveryEntityView.harvestDate}}</span></li>
				</ul>
				<div class="h_i_content">
					<div @click='videoImgClick([recoveryEntityView.photo],0)' v-if='recoveryEntityView.photo!=null&& recoveryEntityView.photo' class="h_i_content_img">
						<img :src="img_url+recoveryEntityView.photo" alt="" />
					</div>
					<div class="h_i_c_bottom">
						<div class="h_i_c_bottom_center">
							<div>检测结果：<span class="hege">{{recoveryEntityView.result}}</span></div>
							<div>检测标准：<span>{{recoveryEntityView.inspectionStandardName}}</span></div>
							<div>检测人：<span>{{recoveryEntityView.qualityStaff}}</span></div>
							<div>检测类型：<span>{{recoveryEntityView.reportTypeName}}</span></div>
						</div>
					</div>
				</div>
			</div>
			<!--加工信息-->
			<div v-if='productInforShow' class="harvest_info process_info">
				<div class="box_header">
					<img src="../assets/img/con_bian.png" alt="" />
					<div class="box_header_name">加工信息</div>
				</div>
				<ul>
					<li>{{productInfor.breedName}}</li>
					<li>规格：<span>{{productInfor.specifications}}</span></li>
					<li>等级：<span>{{productInfor.gradeName}}</span></li>
					<li>负责人：<span>{{productInfor.operatorStaffName}}</span></li>
					<li>生产时间：<span>{{productInfor.operationTime}}</span></li>
				</ul>
				<!--加工信息的生产过程-->
				<div class="process" v-if='productInforListProcessShow'>
					<div class="swiper-container" id='process'>
						<div class="swiper-wrapper">
							<div class="swiper-slide" v-for="(item, index) in productInforListProcess">
								<div class="content_header">加工过程</div>
								<div class='biaoqian'>{{index+1}}</div>
								<ul>
									<li>{{item.name}}</li>
									<template v-if='item.operationDateStr'>
										<li>{{item.operationDateStr.split(' ')[0]}}</li>
									</template>
									<li v-else>时间未上传</li>
									<li class="thr" v-if="item.photo||item.video">
										<div class="swiper-container videoSwiper">
											<div class="swiper-wrapper">
												<div v-if='item.video' class="swiper-slide">
													<!--autoplay="autoplay" controls="controls"-->
													<!--poster="../assets/img/banner.png"-->
													<video id="video1" poster="../assets/img/video.png" controls="controls">
														<source :src="item.video" type="video/mp4" />
														<source id="ogg_src" :src="item.video" type="video/ogg"> 您的浏览器不支持 HTML5 video 标签。
													</video>
												</div>
												<div @click='videoImgClick(item.photo.split(","),index)' v-if='item.photo' v-for='(itemPhoto,index) in item.photo.split(",")' class="swiper-slide">
													<img :data-src="img_url+itemPhoto" alt="" class="swiper-lazy" />
												</div>
											</div>
											<!--<div class="swiper-pagination-video"></div>-->
										</div>
									</li>
									<li class="dian"><i></i>负责人：<span>{{item.operatorStaffName}}</span></li>
									<li class="dian"><i></i>工作参数：<span>{{item.processParameters}}</span></li>
									<li class="dian"><i></i>作业环境：<span>{{item.operatingEnvironment}}</span></li>
									<li class="dian" v-if='item.outdetal.length>0&&item.outdetal!=null'><i></i>原料：<span v-for="(i,n) in item.outdetal">{{i.breedName}}<span v-if='n+1!=item.outdetal.length'>、</span></span>
									</li>
								</ul>
							</div>
						</div>
						<!-- 如果需要分页器 -->
						<div class="swiper-pagination-farm"></div>

					</div>
				</div>
				<div class="look_video" @click='goMonitoring(1)'>查看实时监控</div>
			</div>
			
			<div class="box_5" v-if='isTransShow'>
				<div class="box_header">
					<img src="../assets/img/con_bian.png" alt="" />
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
				<div class="content_header">{{cookbooksName}}</div>
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
		</van-popup>
		<div class="footer" style="background: #E70012;">
			<p class='footerName' style="color: white;">宁夏杞彩回乡土特产开发有限公司</p>
			<!--<p class="jszc" style="color: white;">技术支持：成都九洲电子信息系统股份有限公司</p>-->
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
	import { Toast, Popup, Button, NoticeBar } from 'vant';
	var componentsArr = [Toast, Popup, Button, NoticeBar]
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
				recoveryEntityView: {
					breedName: '',
					gradeName: '',
					harvestDate: '',
					inspectionStandardName: '',
					operatorStaffName: '',
					qualityStaff: '',
					photo: null,
					reportTypeName: '',
					result: '',
				},
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
				vehicleEntity:{},
				isTransShow: false,
				materialBuyList:[],
			}
		},
		computed: {

		},
		mounted() {
			//title判断
			this.titleJudge();
			this.init();
			this.imgSwiper();

		},

		methods: {
			goMonitoring(){
				this.$router.push({
					name:'monitoring',
					query:{
						str:num,
						traceablityNo:this.$route.query.traceablityNo
					}
				})
			},
			yccgSwiper(){
				var swiper = new Swiper('.yccg_swiper', {
						observeParents: true, //修改swiper的父元素时，自动初始化swiper
//						freeMode: true,
						spaceBetween: 20,
						lazyLoading: true,
						lazyLoadingOnTransitionStart: true,
						lazyLoadingInPrevNext: true,
						lazyLoadingInPrevNextAmount: 1,
						autoplay: 4000,
						pagination: 'yccg_pagination',
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
						slidesPerView: 1.28,
						freeMode: true,
						spaceBetween: 20,
						lazyLoading: true,
						lazyLoadingOnTransitionStart: true,
						lazyLoadingInPrevNext: true,
						lazyLoadingInPrevNextAmount: 1,
						autoplay: 4000,
						pagination: paginationName_1,
						paginationType: 'fraction',
					});
					//'.box_2_children'
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
				})

			},
			videoImgClick(imgArr, index, isDz) {
				var vm = this;
				var arr = [];
				var count = null;
				this.showImg = true;
				//判断传下来的东西来进行相应的swiper
				this.showImgObj.removeAllSlides();
				arr = this.videoImgStr(imgArr)
				vm.showImgObj.appendSlide(arr);
				count = arr.length
				vm.showImgObj.slideTo(1);
				vm.showImgObj.slideTo(index);
				for(var i = 0; i < vm.showImgObj.slides.length; i++) {
					vm.showImgObj.slides[i].style.transform = 'translate3d(' + ((count - 1) * 0.5) * vm.showImgObj.width + 'px, 0px, 0px)';
				}
			},
			videoImgStr(imgArr) {
				var arr = [];
				for(let m = 0; m < imgArr.length; m++) {
					var str = `<div  class="swiper-slide">
									<img src=${IMG_URL}${imgArr[m]} alt="" class="swiper-lazy"/>
									</div>`
					arr.push(str)
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
						vm.showImg = false;
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
						if(d.result.materialBuyList!=null&&d.result.materialBuyList){
							this.materialBuyList=d.result.materialBuyList
							this.$nextTick(()=>{
								this.yccgSwiper();
							})
						}
						
						if(this.varietyEntityView.photo != null) {
							this.varietyEntityView.photo = IMG_URL + this.varietyEntityView.photo;
						} else {
							this.varietyEntityView.photo = logo;
							this.imgStyle = {
								width: '1.42rem',
								height: '1.42rem'
							}
						}
						if(d.result.cultureProcessView.cultureEntityView.cultureId == null) {
							this.recoveryEntityViewShow = false; //收获信息
							this.cultureEntityViewShow = false; //档案判断
							this.processEntityListShow = false; //农事过程
						} else {
							this.recoveryEntityViewShow = true;
							this.cultureEntityViewShow = true;
							this.processEntityListShow = true;
							//收获信息
							Object.assign(this.recoveryEntityView, d.result.cultureProcessView.recoveryEntityView)
							if(this.recoveryEntityView.photo != null) {
								this.recoveryEntityView.photo = this.recoveryEntityView.photo
							}
							//档案时间
							Object.assign(this.cultureEntityView, d.result.cultureProcessView.cultureEntityView)
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
							this.vehicleEntity=d.result.vehicleEntity
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
		}
	}
</script>

<style lang="scss">
	@import './../scss/index_1.scss';
</style>