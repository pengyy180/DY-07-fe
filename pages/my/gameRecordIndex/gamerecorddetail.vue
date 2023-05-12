<template>
	<view class="page">
				<u-navbar
					leftIconSize="16px"	
					
		           title="兑换详情"
		           :autoBack="true"
					bgColor="#000"
		       >
		</u-navbar>
		<view class="topher"></view>
		<view class="details">
			<view class="li">
				<view class="left">交易期数: </view>
				<view class="right"> {{details.activityTitile}}</view>
			</view>
			<view class="li">
				<view class="left">交易时间: </view>
				<view class="right"> {{details.createTime}}</view>
			</view>
			<view class="li">
				<view class="left">购买金额: </view>
				<view class="right">{{details.orderPrice}}</view>
			</view>
			<view class="li" v-if="details.zongfanhui > 0">
				<view class="left">中奖金额: </view>
				<view class="right"> {{details.zongfanhui}}</view>
			</view>
			<view class="li">
				<view class="left">打赏主播:</view>
				<view class="right"> 
				{{details.giftDic[0]}}
				{{details.giftDic[1]}}
				{{details.giftDic[2]}}
				{{details.giftDic[3]}}
				</view>
			</view>
			</view>
			
	</view>
</template>

<script>
	export default {
		data() {
			return {
				details:''
			}
		},
		onLoad(option) {
			this.details = JSON.parse(option.datainfo)
			console.log(this.details)
			let gift = this.details.giftDic.split("[")
				 gift = gift[1].split("]")
				 gift = gift[0].split(",")
						if(gift[0] == '1'){
												gift[0] = '玫瑰'
											}else if(gift[0] == '2'){
												gift[0] = '糖心'
											}else if(gift[0] == '3'){
												gift[0] = '火箭'
											}else if(gift[0] == '4'){
												gift[0] = '烟花'
											}
											if(gift[1] == '1'){
												gift[1] = '玫瑰'
											}else if(gift[1] == '2'){
												gift[1] = '糖心'
											}else if(gift[1] == '3'){
												gift[1] = '火箭'
											}else if(gift[1] == '4'){
												gift[1] = '烟花'
											}
					            if(gift[2] == '1'){
												gift[2] = '玫瑰'
											}else if(gift[2] == '2'){
												gift[2] = '糖心'
											}else if(gift[2] == '3'){
												gift[2] = '火箭'
											}else if(gift[2] == '4'){
												gift[2] = '烟花'
											}
					            if(gift[3] == '1'){
												gift[3] = '玫瑰'
											}else if(gift[3] == '2'){
												gift[3] = '糖心'
											}else if(gift[3] == '3'){
												gift[3] = '火箭'
											}else if(gift[3] == '4'){
												gift[3] = '烟花'
											}
					            if(gift == ''){
					              gift = '无'
					            }
								
								this.details.giftDic = 	gift
			
		},
		methods: {
			
		}
	}
</script>

<style lang="scss">
.details{
	width: 100%;
	margin-top: 20px;
	.li{
		width: 95%;
		height: 40px;
		margin: 0 auto;
		display: flex;
		align-items: center;
		justify-content: space-between;
		font-size: 14px;
		color: #fff;
	}
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

