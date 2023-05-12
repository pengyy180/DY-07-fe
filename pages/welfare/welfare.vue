<template>
	<view class="page">
				<u-navbar
		           title="福利"
					leftIconSize="0px"
					bgColor="#16151b"

		       >
		</u-navbar>
		<view class="topher"></view>
		
		<view class="memberlist">
			<view class="li" @click="gotourl('/pages/welfare/movie')">
				<view class="left" >
					<image src="../../static/w1.png"></image> 会员视频
				</view>
				<view class="right">
					骑在身上大屌套弄
					<image src="../../static/19.jpg" mode=""></image>
					<text class="red"></text>
					<text class="iconfont iconjiantou-01"></text>
				</view>
			</view>
			<view class="li" @click="goindex()">
				<view class="left">
					<image src="../../static/w2.png"></image> 新人福利
				</view>
				<view class="right">
					新加入 ID: 087***
					<image src="../../static/18.jpg" mode=""></image>
					<text class="red"></text>
					<text class="iconfont iconjiantou-01"></text>
				</view>
			</view>
			<view class="li" @click="show = true">
				<view class="left">
					<image src="../../static/w3.png"></image> 附近的人
				</view>
				<view class="right">
					为您发现同城炮友 888 位
					<image src="../../static/17.jpg" mode=""></image>
					<text class="red"></text>
					<text class="iconfont iconjiantou-01"></text>
				</view>
			</view>
			<view class="li" @click="gotourl('/pages/welfare/fangpian')">
				<view class="left">
					<image src="../../static/w4.png"></image> 防骗指南
				</view>
				<view class="right">
					<text class="iconfont iconjiantou-01"></text>
				</view>
			</view>
		</view>
		<view class="hz">
			<image class="topimg" src="../../static/w5.png" mode=""></image>
			<view class="list" @click="gotourl('/pages/lottery/lottery')">
				<image src="../../static/w6.png"></image>
				<image src="../../static/w7.png"></image>
			</view>
		</view>
		<u-popup :show="show" mode="center"  @close="close" @open="open" bgColor="transparent">
		        <view class="modalmm" @click="show = false">
		        	<view class="title">
		        		 为您发现同城炮友
		        	</view>
					<view class="content">
						 本平台所有资源真实、安全、可靠、全国空降 选取您心仪的她 随时随地 开启约啪~
					</view>
					<view class="submit" >
						联系专属接待客服
					</view>
		        </view>
		</u-popup>
	</view>
</template>

<script>
	import { findZbjist } from "../../api/gril";
	import __config from "../../config/env.js";
	export default {
		data() {
			return {
				show:false,
				girllist:[],
				action: __config.basePath, // 图片显示地址
			};
		},
		created() {

		},
		methods:{
			gotourl(type){
				console.log(type)
				uni.navigateTo({
				  url: type,
				});
			},
			goindex(){
				uni.switchTab({
					url:'/pages/index/index'
				})
			}
		}
	}
</script>

<style lang="scss">
	.modalmm{
		width: 298px;
		height: 178px;
		background-color: #262626;
		border: 1px solid #7e6a56;
		border-radius: 5px;
		text-align: center;
		display: flex;
		flex-wrap: wrap;
		.title{
			width: 100%;
			font-size: 18px;
			margin-top: 20px;
			font-weight: 700;
			color:	#e6b98a;
		}
		.content{
			width: 80%;
			margin: 0 auto;
			font-size: 14px;
			color:	#fff;
			text-align: left;
			// margin: 10px auto;
		}
		.submit{
			width: 160px;
			height: 22px;
			line-height: 22px;
			font-size: 13px;
			margin: 10px auto;
			border-radius: 30px;
			background: linear-gradient(180deg,#efcba7,#dcab76);
			color: #8a5623;
	
		}
	}
	.hz{
		width: 100%;
		height: 170px;
		margin: 30px auto;
		background: linear-gradient(120deg,#212121,#313131);
		.topimg{
			width: 96px;
			height: 20px;
			margin: 20px 0;
			margin-left: 10px;
		}
		.list{
			display: flex;
			justify-content: space-between;
			align-items: center;
			image{
				width: 160px;
				height: 90px;
				margin: 0 10px;
			}
		}
	}
.memberlist{
	width: 100%;
	.li{
		width: 100%;
		margin: 0 auto;
		display: flex;
		justify-content: space-between;
		align-items: center;
		height: 60px;
		margin-bottom: 1px;
		background: linear-gradient(120deg,#212121,#313131);
		.left{
			margin-left: 10px;
			display: flex;
			align-items: center;
			font-size: 14px;
			color: #fff;
			image{
				width: 25px;
				height: 25px;
				margin-right: 10px;
			}
		}
		.right{
			margin-right: 10px;
			display: flex;
			align-items: center;
			font-size: 12px;
			color: #868686;
			position: relative;
			image{
				width: 40px;
				height: 40px;
				border-radius: 3px;
				margin: 0 5px;
			}
			.red{
				    position: absolute;
					top: -3px;
					right: 12px;
				    width: 8px;
				    height: 8px;
				    background-color: red;
				    border-radius: 50%;
			}
			.iconjiantou-01{
				font-size: 12px;
			}
		}
	}
}
::v-deep .u-navbar__content__title{
	color:	#e6b98a;
}
body{
  width: 100%;
  // height: 100vh;
  // background-color: #282828;
	background-color: #16151b !important;
}
.topher{
	width: 100%;
	height: 44px;
}
</style>
