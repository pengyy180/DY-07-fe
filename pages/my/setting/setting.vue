<template>
	<view class="page">
				<u-navbar
					leftIconSize="16px"	
					
		           title="设置"
		           :autoBack="true"
					bgColor="#282828"
		       >
		</u-navbar>
		<view class="topher"></view>
		<view class="settinglist">
			<view class="li" @click="gotourl('/pages/my/setting/changePassword')">
				<view class="name">修改密码</view>
				<view class="iconright">
					<text class="iconfont iconjiantou-01"></text>
				</view>
			</view>
			<view class="li" @click="gotourl('/pages/my/setting/capitalSettings')">
				<view class="name">设置资金密码</view>
				<view class="iconright">
					<text class="iconfont iconjiantou-01"></text>
				</view>
			</view>
			<view class="li">
				<view class="name">清理缓存</view>
				<view class="iconright">
					<text class="iconfont iconjiantou-01"></text>
				</view>
			</view>
			<view class="li">
				<view class="name">当前版本</view>
				<view class="iconright">
					1.0<text class="iconfont iconjiantou-01"></text>
				</view>
			</view>
			<view class="loginout" @click="onQuitLogin">
				退出登录
			</view>
		</view>
		<DialogBox ref="DialogBox"></DialogBox>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			};
		},
		methods:{
			/**
			 * 退出点击
			 */
			onQuitLogin(){ 
				this.$refs['DialogBox'].confirm({
					title: '提示',
					content: '是否要退出登录?',
					DialogType: 'inquiry',
					animation: 0
				}).then(()=>{
					uni.clearStorage();
					uni.reLaunch({
						url:'/pages/login/login'
					})
					
			
				})
			},
			gotourl(type){
				console.log(type)
				uni.navigateTo({
				  url: type,
				});
			},
		}
	}
</script>

<style lang="scss">
.settinglist{
	width: 95%;
	margin: 0 auto;
	.li{
		width: 100%;
		height: 50px;
		display: flex;
		align-items: center;
		justify-content: space-between;
		font-size: 14px;
		color: #fff;
		border-bottom: 1px solid #fff;
		.iconjiantou-01{
			font-size: 12px;
		}
	}
	.loginout{
		width: 100%;
		height: 50px;
		line-height: 50px;
		text-align: center;
		margin: 20px auto;
		background: linear-gradient(#fde3ca,#e7b486);
		color: #885b3a;
		border-radius: 10px;
	}
}
::v-deep .dialog-box .operation-btn .btn .activity {
    color: #e6b98a !important;
}
::v-deep .u-navbar__content__title{
	color:	#e6b98a;
}
.page{
  width: 100%;
  height: 100vh;
  background-color: #282828;
}
.topher{
	width: 100%;
	height: 44px;
}
</style>
