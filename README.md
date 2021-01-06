# 第一个页面（首页 wxml）

```javascript
<!--index.wxml-->
<view class="container">
   
  <view class="box">
    <view class="text-city">
       <view class="text-city-left">出发城市</view>
       <view class="text-city-left">到达城市</view>
    </view>

    <view class="select-city">
       <view class="select-city-left" bindtap="selectStartCity">{{startCity}}</view>
       <view class="select-city-center" bindtap="qiehuanCity">
         <image class="select-city-center-image" mode= "aspectFit" src="/images/qiehuan.png"></image>
       </view>
       <view class="select-city-right" bindtap="selectEndCity">{{endCity}}</view>
    </view>

    <view class="day-city">
       <view class="day-city-left" bindtap="onDisplay">
         {{date}}
       </view>
       <view class="day-city-right"></view>
    </view>
    <van-cell-group>
    <van-field
               model:value="{{ name }}"
               required
               clearable
               bind:focus="findFocus"
               label="姓名"
               placeholder="请输入姓名"
             />
            </van-cell-group>  

    <view class="search" bindtap="search">
      <van-button color="#F69840" custom-class="button_search" round type="info">查询汽车票</van-button>

    </view>

  </view>

</view>

<van-tabbar active="{{ active }}" bind:change="onChange" active-color="#d81e06">
  <van-tabbar-item icon="home-o">首页</van-tabbar-item>
  <van-tabbar-item icon="search">失物招领</van-tabbar-item>
  <van-tabbar-item icon="friends-o">我的</van-tabbar-item>
</van-tabbar>

<!-- 组件 -->
<van-calendar show="{{ show }}" bind:close="onClose" bind:confirm="onConfirm" />


```

# 第一个页面（首页 js） 
```javascript
//index.js
//获取应用实例
const app = getApp()
import appUtil from '../../utils/util.js';

Page({
    data: {
      url:"",
      active: 0,
      show:false,
      date: '',
      formatDate:'',
      startCity:'昆明',
      endCity:'昆明',
      name:''
    },
    onLoad: function(options) {
     this.getDate(new Date);
     this.index();
    },
    getDate:function(date){
      let data =  this.getCurrentDate(date);
      console.log("获取日期：",data);
      this.setData({
        date:data
      });
    },
    //开始城市选择
    selectStartCity:function(){
      wx.navigateTo({
        url: '/pages/city/city?flag=1',
      })
    },
    //结束城市选择
    selectEndCity:function(){
      wx.navigateTo({
        url: '/pages/city/city?flag=2',
      })
    },
    findFocus:function(event){
      console.log("聚焦出发--------------------------");
      console.log(event.detail.value);
      console.log("聚焦出发--------------------------");
    },
    qiehuanCity:function(){
     var startCity =  this.data.startCity;
     var endCity =  this.data.endCity;
     this.setData({
      startCity:endCity,
      endCity:startCity
     })
    },
    index:function(){
        var that = this;
        // wx.showLoading({
        // title: '加载中',
        // })
     // app.get.globalData.url + "index"
      // wx.request({
      //   url: app.globalData.url + "api/newBookList",
      //   data: {
         
      //   },
      //   success: function (res) {
      //    // console.log("首页获取数据结果"+JSON.stringify(res));
      //     wx.hideLoading();
      //     var arr = res.data.data;
      //     for(var j = 0,len=arr.length; j < len; j++) {
      //       console.log(1);
      //       arr[j].img =   app.globalData.url +arr[j].img
      //     }
      //     that.setData({"books":arr});
      //   },
      // })
    },
    
    getCurrentDate:function (date) {
      var time = appUtil(date,"M月D日");
      var formatDate = appUtil(date,"Y-M-D");
      this.setData({
        formatDate:formatDate
      });
      var week = this.getWeekDate(date);
      return  time + " "+week;
  } ,
  getWeekDate:function (date) {
    var day = date.getDay();
    var weeks = new Array("周日", "周一", "周二", "周三", "周四", "周五", "周六");
    var week = weeks[day];
    return week;
 },
  detail:function(e){
   wx.navigateTo({
     url: '/pages/detail/detail?id=' + e.currentTarget.dataset.aid,
   })
  },
    onReady: function() {
        //Do some when page ready.

    },
    onShow: function() {
        //Do some when page show.
      this.index();
    },
    onHide: function() {
        //Do some when page hide.

    },
    onUnload: function() {
        //Do some when page unload.

    },
    onPullDownRefresh: function() {
        //Do some when page pull down.

    },

  /**
   * 用户点击右上角分享
   */
  onShareAppMessage: function () {

  },
  onDisplay() {
    this.setData({ show: true });
  },
  onClose() {
    this.setData({ show: false });
  },
  onConfirm(event) {
    this.setData({ show: false });
    this.getDate(new Date(event.detail));
  },

  search:function(){
    wx.navigateTo({
      url: '/pages/ticket/ticket?date='+this.data.date+"&formatDate="+this.data.formatDate+"&startCity="+this.data.startCity+"&endCity="+this.data.endCity,
    })
  },
  onChange(event) {
    // event.detail 的值为当前选中项的索引
    if (event.detail == 1){
      wx.reLaunch({
        url: '/pages/list/list',
      })
    }
    if (event.detail == 2) {
      wx.reLaunch({
        url: '/pages/mine/mine',
      })
    }
  }
})
```


# 第二个页面（首页 wxml）

```javascript
<!--pages/ticket/ticket.wxml-->

<view class="top">

  <view class="top-pre">
  </view>
  <view class="top-center"  bindtap="onDisplay">
     <span>{{date}}</span> <van-icon custom-class="arrow" name="arrow-down" />
  </view>
  <view class="top-next">
  </view>
</view>

<view class="content">

   <view class="content-item" wx:for="{{datas}}" wx:key="id" >
     <view class="content-item-left">
        <view class="content-item-left-top">{{item.startTime}}</view>
        <view class="content-item-left-buttom">{{item.roadLength}}</view>
     </view>
     <view class="content-item-right">
      <view class="content-item-right-left">
       <view>
        <van-tag type="primary">始</van-tag>
        <text class="content-item-right-left-chezhan">{{item.startStation}}</text>
       </view>
       <view class="end_car">
        <van-tag type="success">终</van-tag>
        <text class="content-item-right-left-chezhan">{{item.endStation}}</text>
       </view>
       <view class="car_type">
        <image src="/images/bus.png" class="select-city-center-image" mode= "aspectFit"></image>
        <text class="car_type-chezhan">{{item.type}}</text>
       </view>
      </view>  
      <view class="content-item-right-right"> 
         <view class="price">￥{{item.price}}</view>
         <view class="notBuy" wx:if="{{item.seatNum == 0}}">
            已售罄
         </view>
         <view class="notBuy" wx:if="{{item.seatNum != 0 && item.status == 1}}">
            停止售票
         </view>
         <view class="buy" wx:else  bindtap="detail" data-tid="{{item.id}}">预约购票</view>
      </view> 
           
      
         
     </view>
   </view>



 

</view>
<!-- 组件 -->
<van-calendar show="{{ show }}" bind:close="onClose" bind:confirm="onConfirm" />
<van-toast id="van-toast" />

<van-empty wx:if="{{nodatas}}" description="暂无车次" />
```

# 第二个页面（首页 js）

```javascript
// pages/ticket/ticket.js
const app = getApp()
import appUtil from '../../utils/util.js';
import Toast from '../../miniprogram_npm/@vant/weapp/toast/toast';
import Dialog from '../..//@vant/weapp/dialog/dialog';
Page({

  /**
   * 页面的初始数据
   */
  data: {
    startCity:'',
    endCity:'',
    date:'',
    show:false,
    formatDate:'',
    datas:'',
    nodatas:false
  },

  /**
   * 生命周期函数--监听页面加载
   */
  onLoad: function (options) {
  console.log(JSON.stringify(options))
  this.setData(
    {
      startCity:options.startCity,
      endCity:options.endCity,
      date:options.date,
      formatDate:options.formatDate
    }
  );
  this.getDate(new Date(options.formatDate));
  this.getTicket();
  wx.setNavigationBarTitle({
    title: options.startCity+' - '+options.endCity 
  })
  },

  /**
   * 生命周期函数--监听页面初次渲染完成
   */
  onReady: function () {

  },

  /**
   * 生命周期函数--监听页面显示
   */
  onShow: function () {

  },

  /**
   * 生命周期函数--监听页面隐藏
   */
  onHide: function () {

  },

  /**
   * 生命周期函数--监听页面卸载
   */
  onUnload: function () {

  },

  /**
   * 页面相关事件处理函数--监听用户下拉动作
   */
  onPullDownRefresh: function () {

  },

  /**
   * 页面上拉触底事件的处理函数
   */
  onReachBottom: function () {

  },

  /**
   * 用户点击右上角分享
   */
  onShareAppMessage: function () {

  },
  onDisplay() {
    this.setData({ show: true });
  },
  onClose() {
    this.setData({ show: false });
  },
  onConfirm(event) {
    this.setData({ show: false });
    this.getDate(new Date(event.detail));
    this.getTicket();
  },
  getDate:function(date){
    let data =  this.getCurrentDate(date);
    console.log("获取日期：",data);
    this.setData({
      date:data
    });
  },
  getCurrentDate:function (date) {
    var time = appUtil(date,"M月D日");
    var formatDate = appUtil(date,"Y-M-D");
    this.setData({
      formatDate:formatDate
    });
    var week = this.getWeekDate(date);
    return  time + " "+week;
} ,
getWeekDate:function (date) {
  var day = date.getDay();
  var weeks = new Array("周日", "周一", "周二", "周三", "周四", "周五", "周六");
  var week = weeks[day];
  return week;
},
getTicket:function(){
  Toast.loading({
    message: '加载中...',
    forbidClick: true,
  });
  var that = this;
  wx.request({
   url: app.globalData.url + "/api/search",
   data: {
    from:that.data.startCity,
    to:that.data.endCity,
    date:that.data.formatDate
   },
   header:{
    　'content-type': 'application/x-www-form-urlencoded'
  },
   method:"post",
   success: function (res) {
    Toast.clear();
    if (res.data.data && res.data.data.length > 0){
      that.setData({
        datas:res.data.data,
        nodatas:false
      });
    }else{
      that.setData({
        datas:[],
        nodatas:true
      });
    }
    
   },
 })
},
detail:function(e){
  var tid = e.currentTarget.dataset.tid;
  console.log("tid",tid);
  wx.navigateTo({
    url: '/pages/ticket/order?tid='+tid+"&formatDate="+this.data.formatDate+"&date="+this.data.date
  })
}

})
```

