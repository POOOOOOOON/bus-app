<script>
import global from '@/utils/global'
import QQMapWX  from '../static/qqmap/qqmap-wx-jssdk'

export default {
  mounted () {
    var that = this
    const systemInfo = wx.getSystemInfoSync()
    // 胶囊按钮位置信息
    const menuButtonInfo = wx.getMenuButtonBoundingClientRect()
    // 导航栏高度 = 状态栏到胶囊的间距（胶囊距上距离-状态栏高度） * 2 + 胶囊高度 + 状态栏高度
    global.navBarHeight = (menuButtonInfo.top - systemInfo.statusBarHeight) * 2 + menuButtonInfo.height + systemInfo.statusBarHeight
    global.navBarTop = menuButtonInfo.top
    global.menuButtonHeight = menuButtonInfo.height
    global.windowHeight = systemInfo.windowHeight

    global.qqMapSdk = new QQMapWX({
      key: 'L7BBZ-XC5CU-DREVA-B5BI4-HIHH7-U5B6P'  
    })
    // 获取当前定位
    wx.getLocation({
      type: 'wgs84',
      success (res) {
        global.latitude = res.latitude
        global.longitude = res.longitude
        global.qqMapSdk.reverseGeocoder({
          location: {
            latitude: global.latitude,
            longitude: global.longitude
          },
          success (res) {
            global.city = res.result.address_component.city.replace('市','')
          },
          fail: function(error) {
            console.error(error)
          },
          complete: function(res) {
            console.log(res)
          }
        })
      }
    })
  }
}
</script>

<style>
  @import "../colorui/main.css";
  @import "../colorui/icon.css";

  body {
    margin: 0;
    padding: 0;
  }
</style>
