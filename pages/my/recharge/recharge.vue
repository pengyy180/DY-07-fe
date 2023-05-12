<template>
	<view class="page">
				<u-navbar
					leftIconSize="16px"	
					
		           title="充值类型"
		           @rightClick="rightClick"
		           :autoBack="true"
					bgColor="#373536"
					rightText="充值记录"
		       >
		</u-navbar>
		<view class="topher"></view>
		<view class="userauthor">
			<view class="author" >
				<image src="https://zh-green.oss-cn-hongkong.aliyuncs.com/head/en/3.png" mode=""></image>
			</view>
			<view class="name">
				<view class="username">
					用户:{{userinfo.customName}}
				</view>
				<view class="">
					积分:{{userinfo.givenAccount}}元
				</view>
			</view>
			<view class="tips">
				 请联系专属接待或顾问充值积分,请您在付款时确认
			</view>
		</view>
	</view>
</template>

<script>
	import { findCustomData } from "../../../api/index";
	import __config from "../../../config/env.js";
	export default {
		data() {
			return {
				userinfo:'',
				action:__config.basePath,
			};
		},
		created() {
			this.checkuserinfo()
		},
		methods:{
			checkuserinfo(){
				let _this = this
				
				findCustomData().then(res=>{
					_this.userinfo = res.result
						if(res.result.headPortrait == 'https://zh-green.oss-cn-hongkong.aliyuncs.com/head/en/3.png'){
							_this.userinfo.headPortrait = 'https://zh-green.oss-cn-hongkong.aliyuncs.com/head/en/3.png'
						}else{
							_this.userinfo.headPortrait = _this.action+'/'+res.result.headPortrait
						}
				})
			},
			rightClick(){
				uni.navigateTo({
					url:'/pages/my/rechargeRecord/rechargeRecord'
				})
			}
		}
		
	}
</script>

<style lang="scss">
::v-deep .u-navbar__content__title{
	color:	#e6b98a;
} 
::v-deep .u-navbar__content__right__text{
	color:	#fff;
	font-size: 12px;
}
.userauthor{
	width: 100%;
	height: 110px;
	padding: 40rpx 2%;
	display: flex;
	align-items: center;
	justify-content: center;
	flex-wrap: wrap;
	background: linear-gradient(90deg,#e6b98a,#e6b98a);
	.tips{
		width: 100%;
		color: #fff;
		font-size: 12px;
		margin-top: 10px;
	}
	.author{
		width: 20%;
		display: flex;
		align-items: center;
		image{
			width: 110rpx;
			height: 110rpx;

			border-radius: 50%;
		}
	}
	.name{
		width: 75%;
		color: #fff;
		font-size: 14px;
		.username{
			font-size: 14px;
		}
	}
	.iconright{
		color: #fff;
		.iconjiantou-01{
			font-size: 12px;
		}
	}
}
.page{
  width: 100%;
  height: 100vh;
  background-color: #373536;
}
.topher{
	width: 100%;
	height: 44px;
}
</style>
