<template>
  <div>
    <span :class="{active: currentIndex == index}" v-for="(item,index) in 3" :key="index"></span>
  </div>
</template>

<script>
export default {
  name: 'Dot',
  data() {
    return {
      currentIndex: 0,
      timer: null
    }
  },
  beforeDestroy() {
    // 清除定时器
    clearInterval(this.timer)
  },
  mounted() {
    // 开启定时器
    this.timer = setInterval( () => {
      if(this.currentIndex === 2){
        this.currentIndex = 0
      }else{
        this.currentIndex++
      }  
    },500)
  }
}
</script>

<style>
  span {
    display: inline-block;
    width: 10rpx;
    height: 10rpx;
    background-color: #dfe6e9;
    border-radius: 50%;
    margin-left: 10rpx;
  }

  .active{
    background-color: #b2bec3;
  }
</style>