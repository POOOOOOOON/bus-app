<template>
  <div style="background:#f5f6fa">
    <nav-bar navBarTitle="切换城市" :isRetShow="true" />
    <scroll-view class="city-scroll" :style="{'min-height': scrollHeight + 'px'}" :scroll-y="true" :scroll-into-view="toView" :scroll-with-animation="true">
      <view class="cu-bar search bg-white" id="top">
        <view class="search-form round">
          <text class="cuIcon-search"></text>
          <input type="text" placeholder="输入城市名称或拼音" confirm-type="search" v-model="searchContent" @confirm="search"></input>
        </view>
        <view class="action">
          <button class="cu-btn bg-blue.light shadow-blur round sm" @click="search">搜索</button>
        </view>
      </view>
      <div v-if="searching">
        <div v-if="foundCity.length>0">
          <span class="city-label">搜索结果</span>
          <div class="padding-sm bg-white margin-lr radius">
            <div class="margin-tb-sm" v-for="(item,index) in foundCity" :key="index" @click="handleFoundCityClick()">
                <span >{{item}}</span>
            </div>
          </div>
        </div>
        <div v-else style="display:flex;justify-content: center;flex-wrap: wrap;margin-top:50px">
          <img src="/static/images/search.png" style="height:50px;width:50px">
          <div style="width:100%;text-align:center;margin-top:20px">无搜索结果</div>
        </div>
      </div>
      <div v-else>
        <span class="city-label">当前城市</span>
        <div class="city-card" @click="handleCurCityClick">
          <span style="color: #0652DD;margin-left:16px">{{currentCity}}</span>
          <text class="cuIcon-roundcheckfill text-blue margin-lr"></text>
        </div>
        <span class="city-label">定位城市</span>
        <div class="city-card" @click="handlePosCityClick">
          <span style="color: #0652DD;margin-left:16px">{{positionCity}}</span>
          <text v-if="positionCityFlag" class="cuIcon-locationfill text-blue margin-lr"></text>
        </div>
        <span class="city-label">热门城市</span>
        <div class="grid col-4 padding-sm bg-white margin-lr radius">
          <div class="margin-tb-sm text-center" v-for="item in hotCity" :key="item" @click="handleHotCityClick(item.name)">
            <img :src="item.url" style="width:100rpx;height:100rpx;">
            <span style="display:block">{{item.name}}</span>
          </div>
        </div>
        <span class="city-label">全部城市</span>
        <div class="grid col-5 padding-sm bg-white margin-lr radius">
          <div class="margin-tb-sm text-center" v-for="(item,index) in allCity" :key="item" @click="addClassName(index,item.name)">
            <span>{{item.name}}</span>
          </div>
        </div>

        <div v-for="(item1,index1) in allCity" :key="index1" :id="item1.name">
          <span class="city-label">{{item1.name}}</span>
          <div class="padding-sm bg-white margin-lr radius">
            <div class="margin-tb-sm" v-for="(item2,index2) in item1.children" :key="index2" @click="handleAllCityClick(item2.name)">
              <span >{{item2.name}}</span>
            </div>
          </div>
        </div>
      </div>
    </scroll-view>

    <div class='cu-load load-modal' v-if="loading">
      <view class='cuIcon-emojifill text-blue'></view>
      <view class='gray-text'>加载中...</view>
    </div>
  </div>
</template>

<script>
import global from '@/utils/global'
import NavBar from '@/components/navbar/NavBar'

export default {
  name: 'location',
  components: {
    'nav-bar': NavBar
  },
  data() {
    return {
      toView: '',
      scrollHeight: '',
      loading: false,
      searching: false,
      searchContent: '',
      eventChannel: null,
      currentCity: '',
      positionCity: '',
      positionCityFlag: true,
      foundCity: [],
      hotCity: [
        {name: '北京',url: '/static/images/beijing.png'},
        {name: '上海',url: '/static/images/shanghai.png'},
        {name: '广州',url: '/static/images/guangzhou.png'},
        {name: '深圳',url: '/static/images/shenzhen.png'},
        {name: '杭州',url: '/static/images/hangzhou.png'},
        {name: '成都',url: '/static/images/chengdu.png'},
        {name: '天津',url: '/static/images/tianjing.png'},
        {name: '西安',url: '/static/images/xian.png'},
        {name: '青岛',url: '/static/images/qingdao.png'},
        {name: '济南',url: '/static/images/jinan.png'},
        {name: '南京',url: '/static/images/nanjing.png'},
        {name: '武汉',url: '/static/images/wuhan.png'}
      ],
      allCity: [
        {name:'A',children:[]},
        {name:'B',children:[]},
        {name:'C',children:[]},
        {name:'D',children:[]},
        {name:'E',children:[]},
        {name:'F',children:[]},
        {name:'G',children:[]},
        {name:'H',children:[]},
        {name:'J',children:[]},
        {name:'K',children:[]},
        {name:'L',children:[]},
        {name:'M',children:[]},
        {name:'N',children:[]},
        {name:'P',children:[]},
        {name:'Q',children:[]},
        {name:'R',children:[]},
        {name:'S',children:[]},
        {name:'T',children:[]},
        {name:'W',children:[]},
        {name:'X',children:[]},
        {name:'Y',children:[]},
        {name:'Z',children:[]},
      ]
    }
  },
  methods: {
    addClassName(index,id) {
      for (let i = 0; i < this.allCity.length; ++i) {
        if (this.allCity[i].name == id) {
          this.toView = this.allCity[i].name
          break
        }
      }
    },
    handleCurCityClick() {
      this.eventChannel.emit('dataFromLocationPage', this.currentCity)
      wx.navigateBack({})
    },
    handlePosCityClick() {
      if(this.positionCityFlag){
        this.eventChannel.emit('dataFromLocationPage', this.positionCity)
        wx.navigateBack({})
      }
    },
    handleHotCityClick(name) {
      this.eventChannel.emit('dataFromLocationPage', name)
      wx.navigateBack({})
    },
    handleAllCityClick(name) {
      this.eventChannel.emit('dataFromLocationPage', name)
      wx.navigateBack({})
    },
    getPositionCity() {
      var that = this
      global.qqMapSdk.reverseGeocoder({
        location: {
          latitude: global.latitude,
          longitude: global.longitude
        },
        success (res) {
          console.log(res.result)
          that.positionCity = res.result.address_component.city.replace('市','')
        },
        fail: function(error) {
          console.error(error)
        }
      })
    },
    getAllCityList() {
      var that = this
      global.qqMapSdk.getCityList({
        success: function(res) {
          res.result[1].forEach(item => {
            if(item.fullname.indexOf("市") != -1) {
              for(let i in that.allCity) {
                if(that.allCity[i].name === item.pinyin[0].substring(0,1).toUpperCase()) {
                  that.allCity[i].children.push(item)
                  break
                }
              }
            }
          })
          that.loading = false
        },
        fail: function(error) {
          console.error(error)
        }
      })
    },
    search(){
      if(this.searchContent.length === 0)
        return
      this.searching = true
      this.foundCity.length = 0
      const len = this.searchContent.length
      this.allCity.forEach(item =>{
        for(let i in item.children){
          var pinyin = item.children[i].pinyin.join("")
          if(pinyin.substring(0,len) === this.searchContent || item.children[i].name.substring(0,len) === this.searchContent){
            this.foundCity.push(item.children[i].name)
          }
        } 
      })
    }
  },
  mounted() {
    var that = this
    this.toView = 'top'
    that.loading = true
    that.scrollHeight = global.windowHeight - global.navBarHeight + global.tabBarHeight
    that.eventChannel = that.$mp.page.getOpenerEventChannel()
    that.eventChannel.on('acceptDataFromHomePage', (data) => {
      that.currentCity = data
    })
    // 获取当前定位
    wx.getSetting({
      success (res){
        if (res.authSetting['scope.userLocation']) {
          that.getPositionCity()
        }else {
          that.positionCityFlag = false
          that.positionCity = "无法获取当前定位"
        }
      }
    })
    that.getAllCityList()
  }
}
</script>

<style>
  .city-label{
    display: block;
    font-size: 14px;
    color: #636e72;
    margin: 20px 0 16px 20px;
  }

  .city-card{
    display: flex;
    height: 90rpx;
    font-size: 14px;
    margin: 0 16px 0 16px;
    background: #fff;
    border: 1rpx solid #dddee1;
    border-radius: 10px;
    justify-content: space-between;
    align-items: center;

  }
</style>