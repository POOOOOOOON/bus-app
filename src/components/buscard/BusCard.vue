<template>
  <div class="bus-card">
    <div class="bus-info">
      <span style="margin-top:20rpx;font-weight:bold;font-size:16px">{{busNumber}}</span>
      <span style="margin-top:20rpx;">开往 {{busDestination}}</span>
      <span style="margin-bottom:20rpx">候车站 {{busStation}}</span>
    </div>
    <div class="bus-wait-time second-color">
      <text class="cuIcon-wifi lg text-blue" style="position: absolute;top:20rpx;right:30rpx;-webkit-transform: rotate(45deg);"></text>
      <p style="font-size:70rpx;margin-top:30rpx;">{{arrivalTime}}<span style="font-size:14px;">{{unit}}</span></p>
      <span style="display:block">{{distance}}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'BusCard',
  props: {
    busNumber: {
      type: String,
      default: '1路'
    },
    busDestination: {
      type: String,
      default: '宣化路'
    },
    busStation: {
      type: String,
      default: '小村庄'
    },
    arrivalTime: {
      type: String,
      default: '10'
    },
    unit: {
      type: String,
      default: '分钟'
    },
    distance: {
      type: String,
      default: '3站 / 24km'     
    }
  }
}
</script>

<style>
  .bus-card{
    display: flex;
    position: relative;
    margin: 16px 16px 0 16px;
    font-size: 14px;
    overflow: hidden;
    background: #fff;
    border: 1rpx solid #dddee1;
    border-radius: 10px;
    justify-content: space-between;
    box-shadow:0px 0px 5px #dddee1;
  }

  .bus-info span{
    display: block;
    margin-left: 20rpx;
  }

  .bus-wait-time span,p{
    text-align: end;
    margin-right: 20rpx;
  }

  .first-color{
    color: #f0932b;
  }

  .second-color{
    color: #0652DD;
  }

</style>