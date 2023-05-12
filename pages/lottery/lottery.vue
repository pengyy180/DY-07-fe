<template>
	<view class="page">
				<u-navbar
					leftIconSize="16px"	
					
		           title="同城数据"
		           :autoBack="true"
					bgColor="#282828"


		       >
		</u-navbar>
		<view class="topher"></view>
		<fjj-condition ref='condition' @touchmove.stop :color="color" :list="menuList" :defaultValue="defaultValue" @result="resultConditon" />
		<view class="lotterytop">
			<view class="li">
				<view class="title">
					<text>距 第</text>
					<text class="qnum">{{historylist[0].activityTitile}}</text>
					<text>期 截止</text>
					<text class="time">{{downtime[0]}}:{{downtime[1]}}:{{downtime[2]}}</text>
				</view>
				<view class="name" @click="historyshow = !historyshow">
					<text>第</text>
					<text class="qnum">{{historylist[1].activityTitile}}</text>
					<text>期</text>
					<text class="bold">{{historylist[1].tabs[0]}}{{historylist[1].tabs[1]}}</text>
				</view>
			</view>
		</view>
		<u-popup :show="historyshow" mode="right"  @close="close" @open="open">
		        <view class="history"  >
		        	<view class="li"> 
		        		<view>期号</view>
		        		<view>开奖号码</view>	
		        	</view>
		        	<view class="li" v-for="(item,i) in historylist" :key="i"	>
		        		<view>{{item.activityTitile}}</view>
		        		<view>{{item.tabs[0]}}{{item.tabs[1]}}</view>	
		        	</view>
					<uni-load-more :status="status"></uni-load-more>
		        </view>
			</u-popup>
		
		<view class="productlist" >
			<view class="li "
			v-for="(item,i) in buylist" :key="i"
			:class="item.ed == true?'br':''"
			@click="item.ed = !item.ed ,buycount()"
			>
				<view>{{item.name}}</view>
				<view>1.998</view>
			</view>
			
		</view>
		<view class="tipsimg">
			<view class="title">
				<image src="../../static/logo.png" mode=""></image> 南山集团
			</view>
			<view class="contxt">
				本次数据作为直播间打赏主播人气置顶主播排行榜，正式客户完成后可免费开直播间权限
			</view>
			<view class="btn">
				与主播私下约啪
			</view>
		</view>
		<view class="bottom">
			<view class="count">
				<view class="left">
					<text>共 {{buynumber}} 个</text>
					<text>总打赏 {{moneycount}} 元</text>
				</view>
				<view class="right"  @click="isPassword = !isPassword">
					<text>账户余额: {{isPassword ? userinfo.givenAccount : '****'}} 元</text>
					<text
					  class="iconfont"
					  :class="isPassword ? 'icon-eye-on' : 'icon-eye-off'"
					></text>
				</view>
			</view>
			<view class="pay">
				<view class="clear" @click="clearall">
					清空全部
				</view>
				<view class="inputn">
					<input
					  maxlength="26"
					  v-model="buynum"
					  @input="pushapi"
					/>
				</view>
				<view class="payin" @click="buy">
					打赏
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import { findActivityPage,lastAcitiy,buyOrder } from "../../api/gril";
	import {findCustomData } from "../../api/index";
	import fjjCondition from '@/components/fjj-condition/fjj-condition.vue';
	export default {
		components: {
			fjjCondition,
		},
		data() {
			return {
				status: 'no-more',
				historyshow:false,
				buynum:0,
				isPassword: false,
				color: '#e6b98a',
				hasChoose: false,
				menuList: [
					{
					                    'title': '单选',
					                    'type': 'custom',
					                    'key': 'custom2',
					                    'isMutiple': false, //单选
					                    'detailList': [{
					                        title: '选项1',
					                        value: "1",
					                    }, {
					                        title: '选项2',
					                        value: "2",
					                    }],
					 },
				],
				defaultValue: {time1: ['2021/02/01', '2021/02/03']},
				pageNo:1,
				pagesize:30,
				historylist:[],
				downtime:[],
				nowTime:[],
				buylist:[
					{name:'玫瑰',value:'1',ed:false},
					{name:'糖心',value:'2',ed:false},
					{name:'火箭',value:'3',ed:false},
					{name:'烟花',value:'4',ed:false},
				],
				buynumber:0,
				moneycount:0,
				userinfo:'',
				timearray:[]
				
			}
		},
		async onLoad() {
			 await this.historyapi()
			 this.checkuserinfo()
			 this.checkapitime()
		},
		//下拉加载
		onReachBottom() {
			this.pageNo +=1
				this.historyapi();

		},
		mounted(){
		    this.$nextTick(() => { // 动态时间展示
		    	setInterval(this.timeup, 1000);
		    });
		  },
		methods: {
			
			checkapitime(){
				lastAcitiy().then((res)=>{
					this.timearray = res.result
					})
			},
			//倒计时
			  timeup() {
							this.checkapitime()
							  //开始时间
							  let starttime = this.timearray[0].split(' ')
							  starttime = starttime[1].split(':')
							  //当前时间
							  let nowttime = this.timearray[1].split(' ')
							  nowttime = nowttime[1].split(':')
							  //结束时间
							  let endtime = this.timearray[2].split(' ')
							  endtime = endtime[1].split(':')
							  // let start = (new Date(res.result[1])).getTime();
							  let date = new Date()
							  // let start = (new Date(date)).getTime();
							  let start = (new Date(this.timearray[1])).getTime();
							  let end = (new Date(this.timearray[2])).getTime();
							  let downtime = this.getTimem((end - start) / 1000)
							  if(downtime[2] <= 0){
								  this.checkapitime()
								  this.historyapi()
							  }
							   this.downtime = downtime
							  // console.log(downtime,'我到执行的',endtime,nowttime)
			},
			buy(){
				if(this.buynum == 0  ){
					uni.showToast({
						title: '金额不能为0',
						icon:"none",
						duration: 2000
					});
				}else if(this.buynumber == 0 ){
					uni.showToast({
						title: '下单最少选择一个产品',
						icon:"none",
						duration: 2000
					});
				}else if( this.moneycount > this.userinfo.givenAccount ){
					uni.showToast({
						title: '下单金额不能大于用户余额',
						icon:"none",
						duration: 2000
					});
				}else{
					let ai = []
						for (let i = 0; i < this.buylist.length; i++) {
								if(this.buylist[i].ed == true){
									ai.push(this.buylist[i].value)
								}
							}
					buyOrder({account:this.buynum,giftList:ai}).then((res)=>{
						console.log(res)
						
						if(res.success == true){
							uni.showToast({
								title: '下单成功',
								icon:"success",
								duration: 2000
							});
							this.clearall()
						}
					})
				}

				
			},
			clearall(){
				this.checkuserinfo()
				this.buynumber	= 0
				this.moneycount	= 0
				this.buynum = 0
				this.buylist=[
					{name:'玫瑰',value:'1',ed:false},
					{name:'糖心',value:'2',ed:false},
					{name:'火箭',value:'3',ed:false},
					{name:'烟花',value:'4',ed:false},
				]
			},
			pushapi(item){
				this.moneycount = this.buynumber * Number(item.detail.value)
			},
			buycount(){
				let ai = 0
				for (let i = 0; i < this.buylist.length; i++) {
						if(this.buylist[i].ed == true){
							ai += 1
						}
					}
				this.buynumber = ai
				
				this.moneycount = this.buynumber * this.buynum
			},
			
			async historyapi(){

					const  res  = await findActivityPage({pageNo:this.pageNo,pageSize:this.pagesize});
					let newarray = []
					for (let i = 0; i < res.result.records.length; i++) {
						if(res.result.records[i].activityStatus !='3' || res.result.records[i].activityStatus =='0'){
							let gift = res.result.records[i].giftResult.split("[")
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
							// console.log(res.result.records[i].activityStatus)
							 res.result.records[i].tabs = gift

							 newarray.push(res.result.records[i])
						}
						
					
					}
					if(res.result.records.length == 0){
						this.status = 'no-more'
					}

					this.historylist = newarray
			},
			checkuserinfo(){
				let _this = this
				
				findCustomData().then(res=>{
					_this.userinfo = res.result
				})
			
			},
			//筛选返回值
			resultConditon(obj) {
				this.$refs.condition.visibleDrawer = false;
				this.hasChoose = obj.hasChoose;

			},
			rightClick(obj){
				this.$refs.condition.visibleDrawer = true;
			},
			open() {
			        // console.log('open');
					this.historyshow = true
			  },
			  close() {
				this.historyshow = false
				// console.log('close');
			  },
			  //秒转换时分秒
			  getTimem(time) {
			              // 转换为式分秒
			              let h = parseInt(time / 60 / 60 % 24)
			              h = h < 10 ?  h : h
			              let m = parseInt(time / 60 % 60)
			               m = m < 10 ?  m : m
			              let s = parseInt(time % 60)
			               s = s < 10 ?  s : s
			              // 作为返回值返回
			              return [h, m, s]
			  },
		}
	}
</script>

<style lang="scss">
	.tipsimg{
		width: 100%;
		height: 200px;
		margin: 0px auto;
		margin-top: 30px;
		color: #fff;
		background: url('../../static/16.jpg') no-repeat center center;
		.title{
			width: 100%;
			display: flex;
			align-items: center;
			justify-content: center;
			image{
				width: 50px;
				height: 50px;
				margin-top: 10px;
				border-radius: 5px;
				margin-right: 10px;
			}
		}
		.contxt{
			width: 70%;
			margin: 5px auto;
		}
		.btn{
			text-align: center;
			margin: 0 auto;
			color: #e6b98a;
			font-size: 32px;
		}
	}
.history{
	width: 200px;
	height: 100vh;
	background-color: #000;
	.li{
		height: 30px;
		text-align: center;
		font-size: 12px;
		color: #fff;
		display:flex;
		align-items: center;
		view{
			width: 50%;
		}
	}
}
.bottom{
	width: 100%;
	position: fixed;
	bottom: 0px;
	background-color: #7e614a;
	.pay{
		width: 100%;
		height: 40px;
		background-color: #282828;
		display: flex;
		justify-content: space-between;
		align-items: center;
		font-size: 12px;
		.clear{
			width: 20%;
			text-align: center;
			color: #fff;
		}
		.inputn{
			width: 60%;
			input{
				background-color: #7d614a;
				border-radius: 5px;
				text-align: center;
				color: #fff;
			}
		}
		.payin{
			width: 70px;
			background: #e6ac75;
			color: #fff;
			font-size: 14px;
			font-weight: 500;
			height: 30px;
			line-height: 30px;
			text-align: center;
			border-radius: 5px;
		}
	}
	.count{
		width: 95%;
		margin:0 auto;
		height: 40px;
		display: flex;
		justify-content: space-between;
		align-items: center;
		color: #fff;
		font-size: 12px;
		.left{
			text{
				margin-right: 10px;
			}
		}
		.right{
			display: flex;
			align-items: center;
			.iconfont{
				margin-left: 10px;
			}
		}
	}
	
}
.productlist{
	width: 100%;
	display: flex;
	align-items: center;
	flex-wrap: wrap;
	.li{
		width: 164px;
		margin:  10px auto;
		height: 50px;
		background-color: #232128;
		border: 1px solid #232128;
		font-size: 12px;
		display: flex;
		align-items: center;
		justify-content: space-between;
		color: #fff;
		view:nth-child(1){
			font-size: 14px;
			text-indent: 1rem;
		}
		view:nth-child(2){
			 color:#e6b98a;
			 margin-right: 10px;
		}
		
	}
	.br{
		border: 1px solid #f3c7a2;
	}
}
.lotterytop{
	width: 100%;
	background-color: #16151b;
	border-top: 10px solid #836045;
	border-bottom: 10px solid #81644f;
	color: #fff;
	font-size: 12px;
	.li{
		
		.title{
			width: 95%;
			margin:0 auto;
			height: 45px;
			line-height: 45px;
			border-bottom: 1px solid #e6b98a;
			.qnum{
				font-size: 15px;
				margin: 0 5px;
				font-weight: 700;
			}
			.time{
				color: #ff253f;
				font-weight: 500;
				margin-left: 20px;
				font-size: 18px;
			}
		}
		.name{
			width: 95%;
			margin:0 auto;
			height: 45px;
			line-height: 45px;
			.qnum{
				color: #e6b98a;
				margin: 0 5px;
			}
			.bold{
				font-weight: 500;
				margin-left: 10px;
			}
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
  height: 100vh;
  background-color: #282828;
}
.topher{
	width: 100%;
	height: 44px;
}
</style>
