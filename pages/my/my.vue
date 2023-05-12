<template>
	<view class="page">
		<view class="userauthor" >
			<view class="author" >
				<image :src="userinfo.headPortrait" mode=""></image>
			</view>
			<view class="name" @click="gotourl('/pages/my/infomation/infomation')">
				<view class="username">
					用户:{{userinfo.customPhoneNum}}
					<image :src="'../../static/v'+userinfo.customLevel+'.png'" mode=""></image>
				</view>
				<view class="">
					南山集团号:{{userinfo.id.slice(13,19)}}
				</view>
				<view class="">
					剩余观影次数: {{userinfo.playCount}} 次
				</view>
			</view>
			<view class="iconright">
				<text class="iconfont iconjiantou-01"></text>
			</view>
		</view>
		<view class="hrblack"></view>
		<view class="mylist">
			<view class="li"  @click="gotourl('/pages/my/recordCenter/recordCenter')">
				<view class="imgicon">
					<image src="../../static/m1.png" mode=""></image>
				</view>
				<view class="name">
					<text>我的钱包</text>
					<text class="iconfont iconjiantou-01"></text>
				</view>
			</view>
			<view class="hrblack"></view>
			<view class="li libor" >
				<view class="imgicon">
					<image src="../../static/m2.png" mode=""></image>
				</view>
				<view class="name">
					<text>我的客服</text>
					<text class="iconfont iconjiantou-01"></text>
				</view>
			</view>
			<view class="li libor" @click="gotourl('/pages/my/gameRecordIndex/gameRecordIndex')">
				<view class="imgicon">
					<image src="../../static/m3.png" mode=""></image>
				</view>
				<view class="name">
					<text>打赏记录</text>
					<text class="iconfont iconjiantou-01"></text>
				</view>
			</view>
			<view class="li libor" @click="gotourl('/pages/my/rechargeRecord/rechargeRecord')">
				<view class="imgicon">
					<image src="../../static/m4.png" mode=""></image>
				</view>
				<view class="name">
					<text>充值记录</text>
					<text class="iconfont iconjiantou-01"></text>
				</view>
			</view>
			<view class="li" @click="gotourl('/pages/my/withdraw/withdraw')">
				<view class="imgicon">
					<image src="../../static/m5.png" mode=""></image>
				</view>
				<view class="name">
					<text>在线兑换</text>
					<text class="iconfont iconjiantou-01"></text>
				</view>
			</view>
			<view class="hrblack"></view>
			<view class="li libor" @click="gotourl('/pages/my/withdrawRecord/withdrawRecord')">
				<view class="imgicon">
					<image src="../../static/m6.png" mode=""></image>
				</view>
				<view class="name">
					<text>兑换记录</text>
					<text class="iconfont iconjiantou-01"></text>
				</view>
			</view>
			<view class="li libor" @click="gotourl('/pages/my/agentcenter/agentcenter')">
				<view class="imgicon">
					<image src="../../static/m7.png" mode=""></image>
				</view>
				<view class="name">
					<text>援助交际</text>
					<text class="iconfont iconjiantou-01"></text>
				</view>
			</view>
			<view class="li libor" @click="gotourl('/pages/my/message/message')">
				<view class="imgicon">
					<image src="../../static/m8.png" mode=""></image>
				</view>
				<view class="name">
					<text>我的消息</text>
					<text class="iconfont iconjiantou-01"></text>
				</view>
			</view>
			<view class="li libor" @click="gotourl('/pages/my/managebankcard/managebankcard')">
				<view class="imgicon">
					<image src="../../static/m9.png" mode=""></image>
				</view>
				<view class="name">
					<text>银行卡管理</text>
					<text class="iconfont iconjiantou-01"></text>
				</view>
			</view>
			<view class="li" @click="gotourl('/pages/my/setting/setting')">
				<view class="imgicon">
					<image src="../../static/m10.png" mode=""></image>
				</view>
				<view class="name">
					<text>设置</text>
					<text class="iconfont iconjiantou-01"></text>
				</view>
			</view>
		</view>
	</view>
</template>

<script> 
import { updateCustomData,findCustomData,findKefu } from "../../api/index";
	import __config from "../../config/env.js";
	export default {
		data() {
			return {
				userinfo:'',
				action:__config.basePath,
			}
		},
		created() {
			this.checkuserinfo()
		},
		methods: {
			gotourl(type){
				console.log(type)
				uni.navigateTo({
				  url: type,
				});
			},
			checkuserinfo(){
				let _this = this
				
				findCustomData().then(res=>{
					_this.userinfo = res.result
						if(res.result.headPortrait == 'https://zh-green.oss-cn-hongkong.aliyuncs.com/head/en/3.png'){
							_this.userinfo.headPortrait = 'https://zh-green.oss-cn-hongkong.aliyuncs.com/head/en/3.png'
						}else{
							_this.userinfo.headPortrait = _this.action+'/'+res.result.headPortrait
						}
			
			// 			_this.userinfo.givenAccount = _this.handleMoney(_this.userinfo.givenAccount,1)
			// 			_this.userinfo.realAccount = _this.handleMoney(_this.userinfo.realAccount/10,2)
				})
			
			},
		}
	}
</script>

<style scoped lang="scss">
@import "my.scss";
</style>
