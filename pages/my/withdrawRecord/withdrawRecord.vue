<template>
	<view class="page">
				<u-navbar
					leftIconSize="16px"	
					
		           title="兑换记录"
		           @rightClick="rightClick"
		           :autoBack="true"
					bgColor="#000"
					rightText="筛选"
					
		       >
		</u-navbar>
		<fjj-condition ref='condition' @touchmove.stop :color="color" :list="menuList" :defaultValue="defaultValue" @result="resultConditon" />
		<view class="topher"></view>
		<view class="withdrawlist">
			<view class="li" v-for="(item,i) in yelist" @click="gotourl(item)">
				<view class="left">
					<text>兑现积分{{item.accountBigdec}}元</text> <br>
					{{item.createTime}}
				</view>
				<view class="right">
						查看
				</view>
				<view class="iconright">
					{{item.accountStatusname}}<text class="iconfont iconjiantou-01"></text>
				</view>
			</view>
			 
		</view>
	</view>
</template>

<script>
	import {findtixianLis2 } from "../../../api/index";
	import fjjCondition from '@/components/fjj-condition/fjj-condition.vue';
	export default {
		components: {
			fjjCondition,
		},
		data() {
			return {
				color: '#e6b98a',
				hasChoose: false,
				menuList: [
				{
					'title': '审核类型',
					'type': 'custom',
					'key': 'custom1',
					'isMutiple': false, //单选
					'detailList': [{
						title: '同意',
						value: "1",
					}, {
						title: '待处理',
						value: "2",
					}, {
						title: '已驳回',
						value: "3",
					}, {
						title: '已扣押',
						value: "4",
					}],
				},{
					'title': '打款类型',
					'type': 'custom',
					'key': 'custom2',
					'isMutiple': false, //单选
					'detailList': [{
						title: '未打款',
						value: "1",
					}, {
						title: '打款中',
						value: "2",
					}, {
						title: '已打款',
						value: "3",
					}],
				},{
					'title': '日期范围',
					'type': 'range',
					'key': 'time1',
					'minName': 'time1MinName',
					'maxName': 'time1MaxName',
				}],
				defaultValue: {time1: ['2021/02/01', '2021/02/03']},
				yelist:[]
				
			};
		},
		onLoad() {
			
			this.checklist()
		},
		methods: {
			async checklist(index){
				// const  res  = await findtixianList({payType:index,pageNo:this.pageNo,pagesize:this.pagesize})
				const  res  = await findtixianLis2()
					
				this.nowtime = 		new Date(parseInt(res.timestamp)).toLocaleString()
				console.log(res)
					// accountStatus
					res.result.forEach((e)=>{
						if(e.accountStatus == 0){
							e.accountStatusname = '申请中'
						}else if(e.accountStatus == 1){
							e.accountStatusname = '通过'
						}else if(e.accountStatus == 2){
							e.accountStatusname = '拒绝'
						}
						
						
						if(e.accountType == 1){
							e.accountTypename = '充值'
						}else if(e.accountType == 2){
							e.accountTypename = '提现'
						}else if(e.accountType == 3){
							e.accountTypename = '消费'
						}else if(e.accountType == 4){
							e.accountTypename = '返利'
						}else if(e.accountType == 5){
							e.accountTypename = '充值'
						}
					})
					this.yelist = res.result
					
			},
			gotourl(item){
				console.log(item)
				uni.navigateTo({
				  url: '/pages/my/withdrawDetail/withdrawDetail?item='+JSON.stringify(item),
				});
			},
			//筛选返回值
			resultConditon(obj) {
				this.$refs.condition.visibleDrawer = false;
				this.hasChoose = obj.hasChoose;
				console.log(obj);
			},
			rightClick(obj){
				this.$refs.condition.visibleDrawer = true;
			}
		}
	}
</script>

<style lang="scss">
.withdrawlist{
	width: 100%;
	.li{
		width: 95%;
		height: 80px;
		margin: 0 auto;
		display: flex;
		align-items: center;
		justify-content: space-between;
		border-bottom: 1px solid #f2f2f5;
		color: #979799;
		font-size: 14px;
		.left{
			font-size: 12px;
			color: #979799;
			text{
				    color: #fff;
					font-size: 14px;
					margin-bottom: 10px;
					display: inline-block;
			}
		}
		.iconjiantou-01{
			font-size: 12px;
		}
	}
}
::v-deep .uni-drawer__content{
	width: 300px;
	background-color: #282828 !important; 
}	
::v-deep .u-navbar__content__title{
	color:	#e6b98a;
}
::v-deep .u-navbar__content__right__text{
	color:	#fff;
	font-size: 12px;
}
.page{
  width: 100%;
  background-color: #282828;
}
.topher{
	width: 100%;
	height: 44px;
}
</style>
