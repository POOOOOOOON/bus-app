<template>
  <div class="init">
    <img class="logo" src="/static/images/logo.png" alt="">
    <div style="margin-bottom:20rpx">车来了精准实时公交</div>
    <Dot/>
  </div>
</template>

<script>
import Dot from '@/components/dot/Dot'
export default {
  name: 'init',
  components: {
    Dot
  },
  mounted() {
    setTimeout( () => {
      wx.switchTab({
        url: '/pages/home/main'
      })
    },1500)
  },
}
</script>

<style>
  .init{
    display: flex;
    flex-direction: column;
    align-items: center
  }

  .logo{
    width: 125rpx;
    height: 125rpx;
    border: 5rpx solid #fff;
    margin: 200rpx 0 20rpx 0;
  }
</style>