<template>
  <div>
    <nav-bar navBarTitle="车来了精准实时公交" :isRetShow="false" />
    <button v-if="isLogin" class="login" type="primary" open-type="getUserInfo" @getuserinfo="getUserInfo">登陆</button>
    <div v-else class="content" :style="{'min-height':contentHeight+'px'}">
      <div class="padding-tb bg-white">
        <view
          class="cu-avatar xl round margin-left"
          :style="{'background-image':'url('+imgUrl+')'}">
        </view>
        <span class="margin-left" style="font-size:50rpx">{{name}}</span>
      </div>

      <div class="bg-white margin radius">
        <div class="margin-left padding-top-lg">
          <img class="avatar" style="vertical-align: middle" src="/static/images/reward.png" alt="">
          <span class="margin-left-sm" style="font-size:28rpx">打赏</span>
        </div>
        <div class="margin-left padding-top-lg">
          <img class="avatar" src="/static/images/guide.png" alt="">
          <span class="margin-left-sm" style="font-size:28rpx">快捷指南</span>
        </div>
        <div class="margin-left padding-top-lg">
          <img class="avatar" style="vertical-align: middle" src="/static/images/record.png" alt="">
          <span class="margin-left-sm" style="font-size:28rpx">乘车记录</span>
        </div>
        <div class="margin-left padding-top-lg">
          <img class="avatar" src="/static/images/chat.png" alt="">
          <span class="margin-left-sm" style="font-size:28rpx">小车客服</span>
        </div>
        <div class="margin-left padding-tb-lg">
          <img class="avatar" src="/static/images/about.png" alt="">
          <span class="margin-left-sm" style="font-size:28rpx">关于</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import global from '@/utils/global'
import NavBar from '@/components/navbar/NavBar'

export default {
  name: 'profile',
  components: {
    'nav-bar': NavBar
  },
  data() {
    return {
      isLogin: null,
      imgUrl: '',
      name: '',
      contentHeight: null
    }
  },
  mounted() {
    var that = this
    that.contentHeight = global.windowHeight - global.navBarHeight
    wx.getSetting({
      success (res){
        if (res.authSetting['scope.userInfo']) {
          that.isLogin = false
          wx.getStorage({
            key: 'url',
            success (res) {
              that.imgUrl = res.data
            }
          })
          wx.getStorage({
            key: 'name',
            success (res) {
              that.name = res.data
            }
          })
          that.imgUrl = global.avatarUrl
          that.name = global.nickName
        }else{
          that.isLogin = true
        }
      }
    })
  },
  methods: {
    getUserInfo(e){
      // console.log(e.target.userInfo)
      wx.setStorage({
        key: "url",
        data: e.target.userInfo.avatarUrl
      })
      wx.setStorage({
        key: "name",
        data: e.target.userInfo.nickName
      })
      this.imgUrl = e.target.userInfo.avatarUrl
      this.name = e.target.userInfo.nickName
      this.isLogin = false
    }
  }
}
</script>

<style>
  .login{
    width: 60%;
    margin-top: 60%;
  }
  .content{
    background:#f5f6fa;
  }
  .avatar{
    width: 50rpx;
    height: 50rpx;
    vertical-align: middle;
  }
</style>