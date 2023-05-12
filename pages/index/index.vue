<template>
	<view class="page">
				<u-navbar
					leftIconSize="16px"	
					
		           title="南山集团"
				   leftText="全国"
				   leftIcon=""
		           @rightClick="rightClick"
		           :autoBack="false"
						bgColor="#16151b"
						leftIconColor="#e6b98a"

		       >
		       </u-navbar>
			   <view class="main">
			   	
			  
					   <view class="banner">
							<u-swiper
										 :list="list1"
										 height="162px"
										 radius="6px"
								 ></u-swiper>
					   </view>
					   <view class="describe">
							<view class="ul">
								<view class="li">
									<view class="title">南山集团</view>
									<view>华人第一福利品牌</view>
								</view>
								<view class="li">
									为了会员权益，网站只展示<text>1%</text>
									资源并且不定期更新只为展示实力，如需查询预约更多资源请联系上级接待
								</view>
							</view>
							<view class="bottom">
								本平台所有资源真实、安全、可靠，全国空降、同城约炮选取你心仪的她，与您零距离邂逅，快加入我们吧
							</view>
					   </view>
					   <view class="indextab" >
							<view class="li " 
							v-for="(item,i) in indexlist" :key="i" 
							:class="{'active':indextype==i}"
							 @click="indextabchange(i)">
								{{item.name}}
							</view>
					   </view>
					   <view class="describe2" v-if="indextype == 1" >
							<view class="content">
								<view class="lists">
									<view :class="{ top: animate == true }">
										<view class="list" v-for="(item,index) in datalist" :key="index">
											<text>{{item.name}}</text>
											<br>
											{{item.content}}
										</view>
									</view>
								</view>
							</view>
					   </view>
					   <view class="describe2" v-if="indextype == 2" >
							<view class="content">
								<view class="lists">
									<view :class="{ top: animate == true }">
										<view class="list" v-for="(item,index) in pplist" :key="index">
											<text>{{item.name}}</text> <br>
											{{item.content}}
										</view>
									</view>
								</view>
							</view>
					   </view>
					   <view class="describe3" v-if="indextype == 3" >
					   		<view class="name">
					   			平台资源
					   		</view>	
							<view class="content">
								网红、模特、空姐、嫩模、大学生，只有您想不到，没有本平台办不到
							</view>
							<view class="name">
								服务范围
							</view>	
							<view class="content">
								同城约炮，任意地点空降，国内一二线城市当地均有，三线城市也可以通过上级预约安排。
							</view>
					   </view>
					   <view class="indexlist" v-if="indextype == 0">
							<view class="li" v-for="(item,i) in girllist" :key="i" @click="gotourl(item)">
								<view class="left">
									<view class="title">ID:{{item.mmNum}}</view>
									<view class="name">昵称:{{item.mmNikeName}}</view>
									<view class="name">类型:{{item.mmType}}</view>
									<view class="name">所在地区:{{item.mmAddress}}</view>
									<view class="name">服务项目:激活可见</view>
									<view class="icon">
										<u-icon name="eye-fill" color="#929292" size="28"></u-icon>{{item.mmSee}}k
										<text class="iconmagrin"></text>
										<u-icon name="heart-fill" color="#ff8d8c" size="28"></u-icon>{{item.mmLike}}k
									</view>
				 				</view>
								<view class="right">
									<image :src="item.mmPhotos" mode="aspectFill"></image>
								</view>
								<view class="sign-i">
									已认证
								</view>
							</view>
					   </view>
			    </view>
			  
	</view>
</template>

<script>

	import { findMeiMeiList } from "../../api/gril";
	import { findBannerList} from "../../api/index";
	import __config from "../../config/env.js";
	export default {
		
		data() {
			return {
				animate:false,
				timer:null,
				datalist: [],
				pplist:[],
				
				indextype:0,
				indexlist:[
					{name:'南山集团资源'},
					{name:'实施配对'},
					{name:'约炮心得'},
					{name:'服务条款'},
					],
				list1: [
				                    'https://cdn.uviewui.com/uview/swiper/swiper1.png',
				                    'https://cdn.uviewui.com/uview/swiper/swiper2.png',
				                    'https://cdn.uviewui.com/uview/swiper/swiper3.png',
				],
				girllist:[],
				action: __config.basePath, // 图片显示地址
			}
		},
		onLoad() {
			this.timer = setInterval(this.scroll, 3000);
		},
		created() {
			this.girlapi()
			this.createData()
			this.createData2()
		},
		methods: {
			scroll() {
				this.animate = true;
				setTimeout(() => {
					this.datalist.push(this.datalist[0]);
					this.datalist.shift();
					this.pplist.push(this.pplist[0]);
					this.pplist.shift();
					this.animate = false;
				}, 1000);
			},
			createData: function(){
					this.datalist = [
						{name:'叶先生(138****4213)',content:'正在服务中...'},
						{name:'彭先生(139****1251)',content:'正在服务中...'},
						{name:'孙先生(133****4213)',content:'通过本站已预约...'},
						{name:'周先生(166****3415)',content:'正在服务中...'},
						{name:'谢先生(130****8587)',content:'通过本站已预约...'},
						{name:'乐先生(139****6241)',content:'正在服务中...'},
						{name:'江先生(138****4023)',content:'正在服务中...'},
						{name:'田先生(145****6644)',content:'通过本站已预约...'},
						{name:'邓先生(192****6135)',content:'通过本站已预约...'},
						{name:'胡先生(139****6043)',content:'正在服务中...'},
						{name:'高先生(199****4213)',content:'通过本站已预约...'},
						{name:'邱先生(180****4676)',content:'正在服务中...'},
						{name:'董先生(177****8467)',content:'通过本站已预约...'},
						{name:'包先生(175****8632)',content:'正在服务中...'},
						{name:'吴先生(190****5434)',content:'通过本站已预约...'},
					]
					
			},
			createData2: function(){
					this.pplist = [
						{name:'唐先生(138****4135)',content:'本人没让我失望，爱过无悔、流连忘返'},
						{name:'马先生(139****3562)',content:'小姐不错 长得很漂亮 身材很正点 皮肤又白,回味无穷,哈哈'},
						{name:'苏先生(133****6623)',content:'朋友介绍的，妞可以年轻漂亮，服务够莞式'},
						{name:'袁先生(166****5443)',content:'看照片就很心动，没有什么经验。又嫩又滑，年轻就是无敌啊'},
						{name:'昭先生(130****7756)',content:'联系约好时间叫我去她家操她，皮肤还是很紧致'},
						{name:'王先生(139****1412)',content:'服务很赞，推荐，很干净没异味，亲自鉴定'},
						{name:'冯先生(138****7674)',content:'巴适，太巴适了咯'},
						{name:'孙先生(145****7658)',content:'可内射 可射口'},
						{name:'钱先生(192****8867)',content:'叫了一个奶妈，奶水超级多，爽爽爽~~'},
						{name:'赵先生(139****8867)',content:'服务好，还吞精，刺激'},
						{name:'丁先生(199****6654)',content:'叫了个刚生孩子的，还可以喝奶，哈哈~~'},
						{name:'余先生(180****8667)',content:'颜值很高，服务很专业，还会热舞...'},
						{name:'董先生(177****6227)',content:'温柔可人服务好'},
						{name:'毛先生(175****7754)',content:'床上满分 可一字马 性格超好 氵超多'},
						{name:'孔先生(190****5442)',content:'那个妹妹口活真不错，下面还紧'},
						{name:'张先生(132****6245)',content:'很会玩啊，涨姿势啊，感觉白活了二十几年'},
					]
			},
			gotourl(item){
				uni.navigateTo({
					url:'./indexdetails?id='+item.id
				})
			},
			indextabchange(i){
				this.indextype = i;
				this.createData()
			},
			girlapi(){
				let _this = this
				findMeiMeiList().then((res)=>{
					
				
					   
					   for (let i = 0; i < res.result.length; i++) {
					   	
					   	let imglist = res.result[i].mmPhotos.split(",")
					   	res.result[i].mmPhotos = this.action + '/' +imglist[0]

					   }
					   this.girllist = res.result
				})
				//banner图
				findBannerList().then((res)=>{
							  let data = []
							   res.result.forEach(function (item, index) {
								   let li = {}
								   li.url = _this.action+'/'+item.bannerImgSrc
				
									data.push(li)
							   });
							   console.log(data)
								this.list1 = data
				})
			},
			
		}
	}
</script>

<style lang="scss">
	.describe3{
		width: 100%;
		margin: 0 auto;
		height: 150px;
		border-radius: 0.26667rem;
		border: 0.02667rem solid #766350;
		background: linear-gradient(120deg,#212121,#313131);
		display: flex;
		padding: 10px 0px;
		flex-direction: column;
		justify-content: space-between;
		position: relative;
		overflow: hidden;
		.name{
			width: 90%;
			margin: 0 auto;
			color: #e6b98a;
		}
		.content{
			width: 90%;
			margin: 0 auto;
			font-size: 12px;
			color: #fff;
		}
	}
	.describe2{
		width: 100%;
		margin: 0 auto;
		height: 200px;
		border-radius: 0.26667rem;
		border: 0.02667rem solid #766350;
		background: linear-gradient(120deg,#212121,#313131);
		display: flex;
		padding: 10px 0px;
		flex-direction: column;
		justify-content: space-between;
		position: relative;
		overflow: hidden;
		.content{
			display: flex;
			height: 100vh;
			padding: 0 30rpx;
		}
		.lists{
			height: 400rpx;
			overflow: hidden;
			width: 100%;
			.list{
				height: 50px;
				font-size: 12px;
				color: #fff;
				text{
					font-size: 14px;
					color: #e6b98a;
				}
			}
		}
		
		.top {
		    transition: all 1s;
		    margin-top: -100rpx;
		  }
		
	}
	.indexlist{
		width: 100%;
		height: 100%;
		.li{
			    width: 100%;
			    height:150px;
			    border-radius: 10px;
			    border:1px solid #766350;
			    margin-bottom: 10px;
			    background: linear-gradient(120deg,#212121,#313131);
			    display: flex;
			    justify-content: space-between;
				align-items: center;
				position: relative;
				overflow: hidden;
				font-size: 12px;
				.left{
					margin-left: 10px;
					.title{
						background: linear-gradient(90deg,#efcba7,#dcab76);
						color: #8d5825;
						height: 20px;
						line-height: 20px;
						padding: 0 10px;
						border-radius: 20px;
						text-align: center;
						margin-bottom: 10px;
					}
					.name{
						color: #929292;
					}
					.icon{
						color: #929292;
						display: flex;
						margin-top: 10px;
						.iconmagrin{
							margin: 0 5px;
						}
					}
				}
			    .right{
					image{
						width: 100px;
						height: 100px;
						border-radius: 50%;
						margin-right: 10px;
					}
				}
				.sign-i{
						position: absolute;
					    right: -30px;
					    top: 5px;
					    background: #ff6056;
					    font-size: 12px;
					    padding: 5px 30px;
					    font-style: normal;
					    transform: rotate(45deg);
					    color: #fff;
				}
		}
	}
	::v-deep .u-navbar__content__title{
		color:	#e6b98a;
	}
	.indextab{
		width: 100%;
		height: 40px;
		display: flex;
		font-size: 12px;
		margin: 10px 0;
		.li{
			padding: 0 5px;
			margin: 0 5px;
			height: 20px;
			line-height: 20px;
			border-radius:10px;
			background: linear-gradient(90deg,#5b606c,#2b2b2b);
		}
		.active{
			background: linear-gradient(#fde3ca,#e7b486);
			color: #8a5623;
		}
	}
	.page{
		width: 100%;
		// height: 100vh;
		// background-color: #282828;
	}
	.main{
		width: 96%;
		margin:0 auto;
	}
	.banner{
		width: 100%;
		margin: 0 auto;
		margin-top: 44px;
		margin-bottom: 10px;
	}
	.describe{
		width: 100%;
		margin: 0 auto;
		height: 134px;
		border-radius: 0.26667rem;
		border: 0.02667rem solid #766350;
		background: linear-gradient(120deg,#212121,#313131);
		display: flex;
		padding: 10px 0px;
		flex-direction: column;
		justify-content: space-between;
		position: relative;
		overflow: hidden;
		.ul{
			width: 95%;
			height: 83px;
			margin: 0 auto;
			display: flex;
			color: #f3c7a2;
			.li:nth-child(1){
				width: 35%;
				border-right: 3px solid #e6b98a;
				text-align: center;
				font-size: 0.8rem;
				.title{
					font-size: 12px;
					font-weight: 700;
				}
			}
			.li:nth-child(2){
				width: 65%;
				color: #c8c9cc;
				font-size: 12px;
				padding-left: 5px;
				line-height: 1.5rem;
				text{
					font-size: 16px;
					    color: #f8c693;
				}
			}
		}
		.bottom{
			width: 95%;
			margin: 0 auto;
			color: #f3c7a2;
			text-align: center;
			font-size: 0.8rem;
			line-height: 1.5rem;
		}
	}
	.describe:before , .describe:after{
		content: "";
		position: absolute;
		width: 20px;
		height: 300px;
		background: linear-gradient(90deg,rgba(74,70,70,.29),transparent);
		transform: rotate(50deg);
	}
	.describe:after{
		left: 50%;
	}
</style>
