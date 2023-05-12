<template>
  <view class="page">
	<view class="title">
		欢迎来到南山集团
	</view>
    <!-- 填写区 -->
    <view class="input-info">
      <view class="info">
        <input
          type="text"
          maxlength="12"
          v-model="form.phone"
          placeholder="请输入手机号码"
        />

      </view>
      
      <view class="info" :style="isLoginWay ? 'display: none' : ''">
        <input
          :password="!isPassword"
          v-model="form.password"
          maxlength="26"
          placeholder="请输入登录密码"
        />
        <!-- <view class="more">
          <text
            class="iconfont"
            :class="isPassword ? 'icon-eye-on' : 'icon-eye-off'"
            @click="isPassword = !isPassword"
          ></text>
          <text class="mo">忘记密码</text>
        </view> -->
      </view>
    </view>
    <!-- 按钮 -->
    <view class="btn-info">
      <view
        class="btn"
        :style="isLogin ? 'opacity:1' : 'opacity:1'"
        @click="onLogin"
      >
        <text>登录</text>
      </view>
    </view>
    <!-- 操作 -->
    <view class="operation">
    
      <text @click="onRegister" >新用户注册</text>
      <text @click="tips" >忘记密码</text>
    </view>
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
import { login } from "../../api/login";
export default {
  data() {
    return {
      isLogin: false,
      isLoginWay: false,
      isPassword: false,
      // 表单
      form: {
        phone: "",
        // code: "",
        password: "",
      },
    };
  },
  methods: {
    onRegister() {
      uni.navigateTo({
        url: "/pages/register/register",
      });
    },
    tips(){
		uni.showToast({
			title: '请联系顾问或接待员',
			duration: 2000,
			icon:"none"
		});
	},
    /**
     * 登录点击
     */
    onLogin() {
      login(this.form).then((res) => {
        uni.setStorageSync("user", res.result.psCustomMain);
        uni.setStorageSync("token", res.result.token);
        console.log(res);
        if (res.code == 200) {
          uni.reLaunch({
            url: "/pages/index/index",
          });
        }
      });
    },
  },
 
};
</script>

<style scoped lang="scss">
@import "login.scss";
</style>
