<template>
	<view class="page">
				<u-navbar
					leftIconSize="16px"	
		           title="会员视频"
		           :autoBack="true"
					bgColor="#16151b"
		       >
		</u-navbar>
		<view class="topher"></view>
		<view class="video">
		
			    <video id="myVideo" :src="videourl"	 controls></video>
		
		</view>
		<view class="btnawi">
			<view class="left">
				<text>当前播放剩余0次</text> <br>
				<text>点击获取VIP无限播放次数</text>
			</view>
			<view class="right">
				→
			</view>
		</view>
		<view class="jxtitle">
			热门精选
		</view>
		<view class="buylist" @click="gotourl">
			<view class="li">
				<image src="../../static/10.jpg"></image>
				<view>同城数据</view>
			</view>
			<view class="li">
				<image src="../../static/10.jpg"></image>
				<view>空降数据</view>
			</view>
			<view class="li">
				<image src="../../static/10.jpg"></image>
				<view>上门服务</view>
			</view>
			<view class="li">
				<image src="../../static/10.jpg"></image>
				<view>约泡服务</view>
			</view>
		</view>
		<view class="jxtitle">
			猜你喜欢
		</view>
		<view class="movielist">
			<view class="li" v-for="(item,i) in movielist" :key="i" @click="godetails(item)">
				<view class="left">
					<image :src="'http://res.nanshancp.vip/'+item.videoTitle+'.jpg'"></image>
				</view>
				<view class="right">
					<view class="title">{{item.videoTitle}}</view>
					<view>评分:{{(Math.random()*(10-9.5)+9.5).toFixed(1)}}</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import { findVideo,videoCatList,videoList } from "../../api/gril";
	import { findCustomData } from "../../api/index";
	export default {
		data() {
			return {
				videourl:'',
				movielist:''
			}
		},
		onLoad(option){
			this.checkuserinfo()
			this.checkurl(option.id)
			this.typeapi()
		},
		methods: {
			checkurl(id){
				findVideo({videoId:id}).then((res)=>{
					this.videourl = 'http://res.nanshancp.vip/' +  res.result.videoTitle + '.mp4'
					console.log(this.videourl)
				})
			},
			checkuserinfo(){
				let _this = this
				
				findCustomData().then(res=>{
					_this.userinfo = res.result
				})
			
			},
			
			gotourl(){
				uni.navigateTo({
					url:'/pages/lottery/lottery'
				})
			},
			godetails(item){
				uni.navigateTo({
					url:'./moviedetail?id=' +item.id
				})
			},
			async typeapi(){
				
				const  res1  = await videoCatList()
				
				
				var element = Math.floor((Math.random()*res1.result.length));


				const  res2  = await videoList({catId:res1.result[element].id})
				console.log(res2.result)
				this.movielist = res2.result	
			},
			
		}
	}
</script>

<style lang="scss">
	.movielist{
		width: 95%;
		margin: 0 auto;
		.li{
			display: flex;
			height: 100px;
			margin-bottom: 10px;
			.left{
				image{
					width: 100px;
					height: 100px;
					border-radius: 10px;
				}
			}
			.right{
				color: #fff;
				font-size: 12px;
				margin-left: 10px;
				.title{
					display: inline-block;
					color: #fff;
					    font-weight: 700;
						margin-bottom: 40px;
				}
			}
		}
	}
	.buylist{
		width: 95%;
		margin: 0 auto;
		height: 100px;
		display: flex;
		align-items: center;
		justify-content: center;
		border: 1px solid #f3c7a2;
		border-radius: 10px;
		.li{
			width: 25%;
			font-size: 12px;
			color: #fff;
			text-align: center;
			image{
				width: 50px;
				height: 50px;
				border-radius: 50%;
			}
		}
	}
	.jxtitle{
		width: 80px;
		height: 25px;
		border-radius: 20px;
		background: linear-gradient(90deg,#efcba7,#dcab76);
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 12px;
		font-weight: 700;
		color: #8a5623;
		margin:20px 0;
		margin-left: 10px;
	}
	.video{
		width: 100%;
		height: 210px;
		video{
			width: 100%;
			height: 210px;
		}
	}
.btnawi{
	width:320px;
	margin: 20px auto;
	background: #494747;
	display: flex;
	align-items: center;
	// justify-content: space-between;
	height: 40px;
	border-radius: 30px;
	color: #f3c7a2;
	.right{
		width: 30px;
		height: 30px;
		line-height: 28px;
		text-align: center;
		background-color: #f3c7a2;
		border-radius: 50%;
		color: #8a5623;
		display: flex;
		justify-content: center;
	}
	
	.left{
		width: 80%;
		text-align: center;
		text:nth-child(1){
			font-size: 14px;
		}
		text:nth-child(3){
			font-size: 12px;
		}
	}
}
::v-deep .u-search__action{
	color:	#e6b98a;
}
::v-deep .u-navbar__content__title{
	color:	#e6b98a;
}		
::v-deep .u-tabs__wrapper__nav__line{
	left:11px;
}
::v-deep .u-icon__icon .uicon-arrow-left{
	font-size: 36px;
}
.page{
  width: 100%;
  height: 100vh;

}
.topher{
	width: 100%;
	height: 44px;
}
</style>
