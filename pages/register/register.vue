<template>
  <view class="page">
	 		<u-navbar
					leftIconSize="16px"	
					
	            title="注册"
	            @rightClick="rightClick"
	            :autoBack="true"
				bgColor="transparent"
				leftIconColor="#e6b98a"
	        >
	        </u-navbar>
    <!-- 填写区 -->
    <view class="input-info input-info2">
	  <view class="info info2">
	  		  <view class="more">
	  		    <text > *</text>账号名称
	  		  </view>
	    <input
	      type="text"
	      maxlength="12"	
	      v-model="form.phone"
		 
	      placeholder="请输入账号名称"
	    />
	  
	  </view>	
     <!-- <view class="info info2">
		  <view class="more">
		    <text > *</text>手机号码
		  </view>
        <input
          type="tel"
          maxlength="11"
          v-model="form.phone"
          placeholder="请输入手机号码"
        />

      </view> -->
	  <view class="info info2">
	  		  <view class="more">
	  		    <text > *</text>登录密码
	  		  </view>
	    <input
	      type="password"
	      maxlength="12"
	      v-model="form.password"
	      placeholder="请输入登录密码"
	    />
	  
	  </view>
	 <view class="info info2">
	  		  <view class="more">
	  		    <text > *</text>邀请码
	  		  </view>
	    <input
	      type="tel"
	      maxlength="11"
	      v-model="form.code"
	      placeholder="请输入邀请码"
	    />
	  
	  </view>
		
     
    </view>
    <!-- 按钮 -->
    <view class="btn-info">
      <view
        class="btn"
        :style="isLogin ? 'opacity:1' : 'opacity:1'"
        @click="onLogin()"
      >
        <text>注册</text>
      </view>
    </view>
    <!-- 操作 -->
   
	<!-- <img style="margin-top: 100rpx;" src="../../static/styleimg/xinshoutu1.jpeg" alt=""> -->
    <!-- 其他方式登录 -->
    <!-- <view class="other-ways">
      <text>其他登录方式</text>
    </view> -->
    <!-- 登录方式 -->
    <!-- <view class="login-way">
      <view class="way">
        <image src="/static/wx_ico.png" mode=""></image>
        <text>微信登录</text>
      </view>
    </view> -->
  </view>
  <!-- <view><img src="../../static/styleimg/xinshoutu1.jpeg" alt=""></view>
   -->
</template>

<script>
import { register } from "../../api/login";
export default {
  data() {
    return {
      isLogin: false,
      isLoginWay: false,
      isPassword: false,
      // 表单
      form: {
		phone:"",
		customSex:"1",
        code: "6688",
        password: "",
		headPortrait:"https://zh-green.oss-cn-hongkong.aliyuncs.com/head/en/3.png"
      },
    };
  },
  methods: {	
	  checkchina (e) {
		  // form.phone=form.phone.replace(/[^\w\/]/ig,'')
	        console.log(e)
	        this.form.phone = e.detail.value.replace(/[\u4e00-\u9fa5/\s+/]|[`~!@#$%^&*() \\+ =<>?"{}|, \\/ ;' \\ [ \] ·~！@#￥%……&*（）—— \\+ ={}|《》？：“”【】、；‘’，。、_.-:]/g, "")
	      },
    /**
     * 注册点击
     */
    onLogin() {
		
		this.form.phone = this.form.phone.replace(/[^\w\/]/ig,'')
	  if(!this.form.phone  ){
	  	this.showmoadl('请输入账号名称')
	  }else if(!this.form.password ){
	  	this.showmoadl('请输入密码(6-12位)' )
	  }else if(this.form.code != '6688'){
	  	this.showmoadl('您输入的邀请码错误' )
	  }else{
		  register(this.form).then((res) => {
			uni.setStorageSync("user", res.result.psCustomMain);
			uni.setStorageSync("token", res.result.token);
			console.log(res);
			if (res.code == 200) {
			  uni.reLaunch({
				url: "/pages/index/index",
			  });
			}
		  });
	  }
    },
	showmoadl(msg){
		uni.showToast({
			title: msg,
			duration: 2000,
			icon:"none"
		});
	},
  },
 
};
</script>

<style scoped lang="scss">
@import "../login/login.scss";
::v-deep .u-navbar__content__title{
	color:	#e6b98a;
}
</style>
