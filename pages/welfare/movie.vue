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
		<view class="tablist">
			 <u-tabs :list="list1" 
				lineWidth="50"
				lineHeight="3px"
				lineColor="#e6b98a"
				keyName="catName"
				@change="typechage"
				:activeStyle="{
					color: '#e6b98a',
					fontWeight: 'bold',
					transform: 'scale(1.05)'
				}"
				:inactiveStyle="{
					color: '#fff',
					transform: 'scale(1)'
				}"
				itemStyle="padding-left: 15px; padding-right: 15px; height: 34px;"
			 ></u-tabs>
		</view>
		<view class="movielist">
			<view class="li" v-for="(item,i) in movielist" :key="i" @click="gotourl(item)">
				<image :src="'http://res.nanshancp.vip/'+item.videoTitle+'.jpg'"></image>
				<view class="num">999播放次</view>
				<view class="title">{{item.videoTitle}}</view>
			</view>
			
		</view>
		<uni-load-more :status="status"></uni-load-more>
	</view>
</template>

<script>
	import { videoCatList,videoList } from "../../api/gril";
	import __config from "../../config/env.js";
	export default {
		data() {
			return {
				 list1: [],
				 movielist:[],
				 status: 'no-more',
				 pageSize:2,
				 pageNo:1,
				 typeid:0,
				 action: __config.basePath, // 图片显示地址
				
			}
		},
		created() {
			this.typeapi()
		},
		onReachBottom() {
		  console.log(12333);
		},
		methods: {
			typechage( index){
				// console.log(index)
				this.listapi(index.id)
			},
				async typeapi(){
					await videoCatList().then((res)=>{
						this.list1 = res.result
						this.typeid = res.result[0].id
						// console.log(this.typeid)
						this.listapi(this.typeid)
					})
				},
				//下拉加载
				onReachBottom() {
					this.pageNo +=1
						this.funFindProductList(this.typeid);
					uni.$u.toast("上拉触发",this.pageNo)
					//这是上拉触发的函数.一般在这里进行分页操作. 
				    /*会有一种情况就是当数据已经全部加载完成了,再次触底还会调用接口,
				      这时就需要去记录一下是否已经全部加载完毕,加载完毕就不去请求接口了.
				      这时最常见的节流场景*/
				},
				async listapi(typeid){
					
					
					const  res2  = await videoList({catId:typeid,pageNo:this.pageNo,pageSize:this.pageSize})
						// for (let i = 0; i < res2.result.records.length; i++) {
							
						// 	// let imglist = res2.result.records[i].productImgs.split(",")
						// 	// res2.result.records[i].productImgs = this.action + '/' +imglist[0]
						// 	// res2.result.records[i].startnumber = (Math.random()*(5-4.5)+4.5).toFixed(1)
						// }
						// if(res2.result.records.length == 0){
						// 	this.status = 'no-more'
						// }
						console.log(res2.result)
						this.movielist = res2.result	
				},
				gotourl(item){
					uni.navigateTo({
						url:'./moviedetail?id=' +item.id
					})
				}
		}
	}
</script>

<style lang="scss">
	.tablist{
		margin: 10px auto;
	}
.movielist{
	width: 100%;
	display: flex;
	flex-wrap: wrap;

	justify-content: space-between;
	.li{
		width: 175px;
		margin: 0 auto;
		height: 100%; 
		color: #fff; 
		font-size: 12px; 
		margin-bottom: 20px; 
		position: relative; 
		.title{
			// height: ;
		}
		image{
			width: 100%;
			height: 110px;
			border-radius: 10px;
		}
		.num{
			position: absolute;
			bottom: 60px;
			left: 5px;
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
