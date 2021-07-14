<template>
  <div>
    <nav-bar navBarTitle="车来了精准实时公交" :isRetShow="false" />
    <view class="action" @click="handleLocation">
      <text class="city">{{currentCity}}</text>
      <text class="cuIcon-triangledownfill"></text>
    </view>
    <div style="background:#f5f6fa">
      <view class="cu-bar search bg-white">
        <view class="search-form round">
          <text class="cuIcon-search"></text>
          <input type="text" placeholder="搜索公交线路、车站、地点" confirm-type="search"></input>
        </view>
      </view>
      <i-tabs :current="currentTab" scroll @change="handleTabSelect">
        <i-tab key="tab1" title="推荐"></i-tab>
        <i-tab key="tab2" title="收藏"></i-tab>
        <i-tab key="tab3" title="历史"></i-tab>
      </i-tabs>
      <div v-if="currentTab === 'tab1'">
        <bus-card></bus-card>
        <bus-card></bus-card>
        <bus-card></bus-card>
        <bus-card></bus-card>
        <bus-card></bus-card>
        <bus-card></bus-card>
        <bus-card></bus-card>
      </div>
      <div v-else-if="currentTab === 'tab2'" class="null-card">
        没有收藏线路
      </div>
      <div v-else class="null-card">
        没有历史线路
      </div>
    </div>  
  </div>
</template>

<script>
import global from '@/utils/global'
import NavBar from '@/components/navbar/NavBar'
import BusCard from '@/components/buscard/BusCard'

export default {
  name: 'home',
  components: {
    'nav-bar': NavBar,
    'bus-card': BusCard
  },
  data() {
    return {
      currentTab: 'tab1',
      currentCity: ''
    }
  },
  mounted() {
    this.currentCity = global.city

    const systemInfo = wx.getSystemInfoSync()
    global.windowHeight = systemInfo.windowHeight
    global.tabBarHeight = ( systemInfo.screenHeight - systemInfo.windowHeight - systemInfo.statusBarHeight ) * systemInfo.pixelRatio
  },
  methods: {
    handleTabSelect(event) {
      this.currentTab = event.mp.detail.key
    },
    handleLocation() {
      var that = this
      wx.navigateTo({
        url: '/pages/location/main',
        events: {
          // 为指定事件添加一个监听器，获取被打开页面传送到当前页面的数据
          dataFromLocationPage (data) {
            that.currentCity = data
          }
        },
        success(res) {
          // 通过eventChannel向被打开页面传送数据
          res.eventChannel.emit('acceptDataFromHomePage', that.currentCity)
        }
      })
    }
  }
}
</script>

<style>
  .city{
    font-size: 50rpx;
    font-weight: bold;
    margin-left: 30rpx;
  }

  .null-card{
    text-align: center;
    height: 160rpx;
    line-height: 160rpx;
    margin: 16px 16px 16px;
    font-size: 14px;
    background: #fff;
    border: 1rpx solid #dddee1;
    border-radius: 10px;
  }
</style>
