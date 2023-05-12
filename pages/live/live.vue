<template>
	<view class="page">
				<u-navbar					
		           title="直播间"
					leftIconSize="0px"
					bgColor="transparent"

		       >
		</u-navbar>
		<view class="topher"></view>
		<view class="mmlist" v-for="(item,i) in girllist" :key="i">
			<view class="mmli" >
				<view class="content">
					<view class="left">
						<image class="grilimg"	:src="action + '/' + item.mmAvatar"></image>
						<image class="livegif" src="../../static/livem.gif"></image>
					</view>
					<view class="right">
						<view class="name">{{item.mmNikeName}} <image src="../../static/livesex.png"></image></view>
						<view class="name2">官方认证女神</view>
						<view class="name3">已缴纳20000保证金</view>
						<view class="name">地区:{{item.mmAddress}}</view>
					</view>
				</view>
				<view class="video">
					<view class="left" @click="gotourl(item)">
						<image :src="action + '/' + item.mmAvatar"></image>
						<view class="play"> <b></b>	</view>
					</view>
					<view class="right">
						<view class="qm">签名:{{item.mmSign}}</view>
						<view class="tabslist" >
							<view class="li" v-for="(item2,i2) in item.mmTags" :key="i2"	>	
								<text> {{item2}}</text>
							</view>
						</view>
						<view class=	"plan" @click="show = true">
							<image src="../../static/liveplan.png"></image>获取联系方式
						</view>
					</view>
				</view>
			</view>
		</view>
		<u-popup :show="show" mode="center"  @close="close" @open="open" bgColor="transparent">
		        <view class="modalmm" @click="show = false">
		        	<view class="title">
		        		温馨提示
		        	</view>
					<view class="content">
						联系数据老师激活权限
					</view>
					<view class="submit" >
						确定
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
			this.checkgirl()
		},
		methods:{
			gotourl(item){
					uni.navigateTo({
						url:'./video?url=' + JSON.stringify(item.videoUrl)
					})
			},
			checkgirl(){
				findZbjist().then((res)=>{
					let tablist = []
					for (let i = 0; i < res.result.length; i++) {
						
						res.result[i].mmTags = res.result[i].mmTags.split(",")
						// tablist = tablist.push(tabname)


					}
					
					this.girllist = res.result

					
				})
			},
			open() {
			        // console.log('open');
			  },
			  close() {
				this.show = false
				// console.log('close');
			  }
		}
	}
</script>

<style lang="scss">
	.modalmm{
		width: 278px;
		height: 158px;
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
			width: 100%;	
			font-size: 15px;
			color:	#fff;
			// margin: 10px auto;
		}
		.submit{
			width: 160px;
			height: 22px;
			margin: 10px auto;
			border-radius: 30px;
			background: linear-gradient(180deg,#efcba7,#dcab76);
			color: #8a5623;

		}
	}
.mmlist{
	width: 95%;
	height: 275px;
	margin: 0px auto;
	margin-bottom: 10px;
	border-radius: 10px;
	background: linear-gradient(120deg,#212121,#313131);
	.video{
		width: 95%;
		margin: 0px auto;
		display: flex;
		align-items: center;
		.tabslist{
			display: flex;
			align-items: center;
			flex-wrap: wrap;
			margin-bottom: 10px;		
			.li{
				width: 50%;
				color: #fff;
				margin-bottom: 5px;
				text{
					padding: 0.5px 10px;
					border-radius: 10px;
				}
				text:first-child {
					background-color: #e9335d;
				}
				
			}
			.li:nth-child(2) text{
					background-color: #d14a42;
			}
			.li:nth-child(3) text{
					background-color: #285ab7;
			}
			.li:nth-child(4) text{
					background-color: #eb46b9;
			}
		}
		.qm{
			color: #c0c0c0;
			font-size: 13px;
			margin-bottom: 10px;
		}
		.right{
			font-size: 12px;
			margin-left: 10px;
			.plan{
					width: 160px;
					height: 30px;
				    text-align: center;
				    border-radius: 30px;
				    background-image: linear-gradient(90deg,#e34c6f,#ef9163);
				    display: flex;
				    align-items: center;
					color: #fff;
					font-size: 14px;
					image{
						width: 16px;
						height: 16px;
						margin-right: 10px;
					}
				    justify-content: center;
			}
		}
		.left{
			position: relative;
			.play{
				position: absolute;
				top: calc(40%);
				left: calc(40%);
				width: 36px;
				height: 36px;
				border-radius: 50%;
				background-color: rgba(0,0,0,.3215686274509804);
				b{
					content: "";
					display: inline-block;
					position: absolute;
					top: 18px;
					left: 35px;
					transform: translate(-50%,-50%);
					height: 0px;
					width: 20px;
					border-style: solid;
					border-width: 10px;
					border-color: transparent transparent transparent #fff;
					vertical-align: middle;
					z-index: 999;
				}
			}
			image{
				width: 127px;
				height: 165px;
				border-radius: 10px;
			}
			
		}
		
	}
	.content{
		width: 100%;
		height: 90px;
		margin: 10px auto;
		display: flex;
		align-items: center;
		.right{
			width: 60%;
			font-size: 14px;
			margin-left: 30px;
			font-weight: 700;
			.name{
				display: flex;
				align-items: center;
				color: #fff;
			}
			.name2{
				color: #7ef0f4;
			}
			.name3{
				color:	#e86272;
			}
			image{
				width: 15px;
				height: 13px;
				background-color: #e5338b;
				padding: 2px;
				border-radius: 2px;
				margin-left: 100px;
			}
		}
		.left{
			position: relative;
			display: flex;
			align-items: center;
			justify-content: center;
			margin-left: 30px;
			.grilimg{
				width: 75px;
				height: 75px;
				border-radius: 50%;
			}
			.livegif{
				width: 20px;
				height: 20px;
				position: absolute;
				right: -10px;
				top: 50%;
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
