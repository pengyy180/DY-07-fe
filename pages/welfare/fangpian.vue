<template>
	<view class="page">
				<u-navbar
					leftIconSize="16px"	
					
		           title="防骗指南"
		           @rightClick="rightClick"
		           :autoBack="true"
					bgColor="#000"
	
		       >
		</u-navbar>
		<view class="topher"></view>
		<view class="topimg">
			<image src="../../static/fpzn.png" mode=""></image>
		</view>
		<view class="content">
			<view class="left">
				<view class="li">
					1.本平台所有资源均为实名制，小姐姐与小哥哥皆是VIP会员请对双方需一定的尊重，请不要出言不逊没有素质，经发现将取消约炮资格请谨记！，重要的事情说三遍！！！
				</view>
				<view class="li">
					2.小姐姐是不会收取任何费用，只需完成平台三次打赏任务获得约炮名额即可享受同城约炮资源，开房花销需自理哦！
				</view>
				<view class="li">
					3.妹子已到楼下,要求付【全款才能上楼】均为骗子无疑，请立即来平台联系接待客服举报！
				</view>
			</view>
			<view class="right">
				<image src="../../static/fp1.png" mode=""></image>
			</view>
		</view>
		<view class="content2">
			<view class="left">
				<image src="../../static/fp2.png" mode=""></image>
			</view>
			<view class="right">
				<view class="li tips">
					友情提醒：老师，接待员，派单员，都不会私加客户的联系方式，上班时间均为 14.00~2.00 其余时间均不办理业务，若有在下班时间他人给你发送信息请不要相信避免资金受损我方概不负责！！！
				</view>
				<view class="li">
					4.凡是要求下载其他软件的都是骗子,请立即截图来平台联系接待客服举报！
				</view>
				<view class="li">
					5.本平台对每一位VIP会员寻欢经历负责,遇到任何问题请及时来平台联系接待客服举报,官方核实后退还解锁消耗!
				</view>
			</view>
			
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
		}
			
	}
</script>

<style lang="scss">
	.content2{
		width: 98%;
		margin: 10px auto;
		display: flex;
		.left{
			image{
				width: 100px;
				height: 200px;
				margin: 0 10px;
			}
		}
		.right{
			width: 275px;
			.li{
				font-size: 14px;
				color: 000;
				font-weight: 600;
				margin-bottom: 10px;
			}
			.tips{
				color: #e6b98a;
			}
		}
	}
	.content{
		width: 98%;
		margin: 10px auto;
		display: flex;
		.left{
			width: 275px;
			.li{
				font-size: 14px;
				color: 000;
				font-weight: 600;
				margin-bottom: 10px;
			}
		}
		.right{
			image{
				width: 80px;
				height: 200px;
				margin: 0 10px;
			}
		}
	}
	.topimg{
		width: 100%;
		display: flex;
		align-items: center;
		image{
			width: 320px;
			height: 95px;
			margin: 0px auto;
		}
	}
::v-deep .u-navbar__content__title{
	color:	#e6b98a;
}
.page{
  width: 100%;
  height: 100vh;
  background-color: #fff;
}
.topher{
	width: 100%;
	height: 44px;
}
</style>
