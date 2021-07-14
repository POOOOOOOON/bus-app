<template>
  <div :style="{'margin-top': navBarHeight + 'px'}">
    <div class="nav-bar" :style="{ height: navBarHeight + 'px' }">
      <i-icon class="margin-left" v-if="isRetShow" @click="returnClick" type="return" size="26" color="#80848f"  :style="{'margin-top': navBarTop + 'px',float: 'left !important'}"/>
      <span class="nav-bar-title" :style="{'margin-top': navBarTop + 'px',height: menuButtonHeight + 'px','line-height': menuButtonHeight + 'px'}">{{ navBarTitle }}</span>
    </div>
  </div>
</template>

<script>
import global from '@/utils/global'

export default {
  name: 'NavBar',
  props: {
    navBarTitle: {
      type: String,
      default: '车来了精准实时公交'
    },
    isRetShow: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      navBarHeight: global.navBarHeight,
      navBarTop: global.navBarTop,
      menuButtonHeight: global.menuButtonHeight
      // windowHeight: global.windowHeight
    }
  },
  methods: {
    returnClick() {
      wx.navigateBack({})
    }
  },
}
</script>

<style>
  .nav-bar{
    position: fixed;
    left: 0;
    right: 0;
    top: 0;
    background-color: #fff !important;
    text-align: center;
    z-index: 999;
  }

  .nav-bar-title{
    display: inline-block;
    font-weight: bold;
  }
</style>