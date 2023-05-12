<template>
	<view class="page">
				<u-navbar
					leftIconSize="16px"	
					
		           title=""
		           :autoBack="true"
					bgColor="transparent"
		       >
		</u-navbar>
		
		<view class="banner">
			<u-swiper
						 :list="imgdata"
						 height="486px"

				 ></u-swiper>
		</view>
		<view class="mnotice">
			<view class="left">
				<image src="../../static/indexleft.png" mode=""></image>
			</view>
			<view class="right">
				公告
			</view>
		</view>
		<view class="mname">Id: {{mmdetails.mmNum}}**</view>
		<view class="mname">激活可见</view>
		<view class="mmdf">
			<view class="left">
				<view>南山集团打分</view>
				<view class="fs">{{mmdetails.mmFraction}}</view>
				<view>美女颜值</view>
			</view>
			<view class="right">
				<view>详细地址：已隐藏，联系小蜜激活可见</view>
				<view>联系方式：已隐藏，联系小蜜激活可见</view>
			</view>
		</view>
		<view class="mmxx">
			<view class="li"></view>
			<view class="li"></view>
			<view class="li">
				<image src="../../static/index1.png"></image>基本信息 {{mmdetails.mmNikeName}} {{mmdetails.mmAge}}岁
			</view>
			<view class="li">
				<image src="../../static/index2.png"></image>所在地区 {{mmdetails.mmAddress}}
			</view>
			<view class="li">
				<image src="../../static/index3.png"></image>服务项目 激活可见
			</view>
			<view class="lib">
				祝南山集团的会员们，天天约得美娇娘，夜夜都能当新郎！
			</view>
		</view>
		<view class="mmimglist" v-for="(item,i) in imgdata" :key="i">
			<image :src="item"></image>
		</view>
		<view class="mmyh" @click="tomodal">
			立即约她
		</view>
	</view>
</template>

<script>
	import { findMeiMei } from "../../api/gril";
	import __config from "../../config/env.js";
	export default {
		data() {
			return {
				 mmdetails:'',
				action: __config.basePath, // 图片显示地址
				imgdata:[]
			}
		},
		onLoad(option) {
			this.checkmm(option.id)
		},
		methods: {
			checkmm(id){
				findMeiMei({meimeiId:id}).then((res)=>{
					
						this.mmdetails = res.result
						let imglist = res.result.mmPhotos.split(",")

					let imgdata = []	
					for (let i = 0; i < imglist.length; i++) {
						imgdata.push(this.action + '/' +imglist[i])
					}
					this.imgdata = imgdata

					// this.girllist = res.result
				})
			},
			tomodal(){
				uni.showToast({
					title: '请联系专属客服接待',
					icon:"none",
					duration: 2000
				});
			}
		}
	}
</script>

<style lang="scss">
	.mmyh{
		width: 70%;
		margin: 20px auto;
		background: linear-gradient(#fde3ca,#e7b486);
		    text-align: center;
			color: #333;
			font-size: 14px;
			height: 36px;
			line-height: 36px;
			border-radius: 30px;
	}
	.mmimglist{
		width: 95%;
		margin: 20px auto;
		image{
			width: 100%;
			height: 493px;
		}
	}
.mmxx{
	width: 95%;
	margin: 20px auto;
	height: 170px;
	border: 1px solid #e6b98a;
	border-radius:5px;
	font-size: 12px;
	color: #fff;
	.li{
		width: 95%;
		margin: 10px auto;
		display: flex;
		align-items: center;
		image{
			width: 20px;
			height: 20px;
			margin-right: 5px;
		}
	}
	.lib{
		height: 40px;
		line-height: 40px;
		width: 95%;
		margin: 10px auto;
		border-top: 1px solid #e6b98a;
	}
}
.mmdf{
	width: 95%;
	margin: 0px auto;
	height: 80px;
	border: 1px solid #e6b98a;
	border-radius:5px;
	display: flex;
	justify-content: space-between;
	align-items: center;
	font-size: 12px;
	color: #fff;
	.left{
		margin-left: 10px;
		text-align: center;
		.fs{
			color: #f3c7a2;
			font-size: 20px;
			font-weight: 700;
		}
	}
	.right{
		margin-right: 10px;
	}
	
}
.mname{
	width: 95%;
	margin: 10px auto;
	font-size: 12px;
	color: #fff;
}
.mnotice{
	width: 95%;
	margin: 0 auto;
	height: 30px;
	border: 1px solid #e6b98a;
	border-radius:5px;
	display: flex;
	justify-content: space-between;
	align-items: center;
	.left{
		display: flex;
		align-items: center;
		image{
			width: 20px;
			height: 20px;
			margin-left: 10px;
		}
	}
	.right{
		margin-right: 10px;
		text-align: center;
		font-size: 12px;
		color: #533921;
		text-indent: 1rem;
		width: 75px;
		height: 20px;
		line-height: 20px;
		background: url('../../static/indexright.png');
	}
}
.banner{
		width: 100%;
		margin: 0 auto;
		// margin-top: 44px;
		margin-bottom: 10px;
	}
.page{
  width: 100%;
  // height: 100vh;
  background-color: #282828;
}
// .topher{
// 	width: 100%;
// 	height: 44px;
// }
</style>
