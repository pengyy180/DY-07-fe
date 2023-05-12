<template>
	<view class="page">
				<u-navbar
					leftIconSize="16px"	
					
		           title=""
		           :autoBack="true"
					bgColor="transparent"

		       >
		</u-navbar>
		<view class="video">

			    <video id="myVideo" :src="videourl"	enable-danmu danmu-btn controls></video>

		</view>
	
	</view>
</template>

<script>

	export default {
		data() {
			return {
				videourl:'',
			};
		},
		onLoad(option) {
			this.videourl = JSON.parse(option.url)
			console.log(this.videourl)
		}
	}
</script>

<style lang="scss">
.video{
	width: 100%;
	height: 100vh;
	video{
		width: 100%;
		height: 100vh;
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
