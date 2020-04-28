> ### 微信小程序的组件

常用组件

- cover-image，cover-view

- scroll-view

- swiper

- 表单组件

- 媒体组件

  - camera

  - video

  - live-player 实时音频播放

  - live-pusher 实时音视频录制

- 地图

  - map

- canvas

原生组件 小程序中的部分组件是由客户端创建的原生组件，这些组件有：

- camera

- canvas

- input（仅在 focus 时表现为原生组件）

- live-player

- live-pusher

- map

- textarea

- video

> ### 微信小程序相关文件类型

- WXML

  框架设计的一套标签语言,结合基础组件、事件系统，可以构建出页面的结构。内部主要是微信自己定义的一套组件.

- WXSS

  是一套样式语言，用于描述 WXML 的组件样式

- js

  负责小程序的逻辑处理，网络请求

- WXS

  是小程序的一套脚本语言，结合 WXML，可以构建出页面的结构。

  1. wxs 不依赖于运行时的基础库版本，可以在所有版本的小程序中运行。
  2. wxs 与 javascript 是不同的语言，有自己的语法，并不和 javascript 一致。
  3. wxs 的运行环境和其他 javascript 代码是隔离的，wxs 中不能调用其他 javascript 文件中定义的函数，也不能调用小程序提供的 API。
  4. wxs 函数不能作为组件的事件回调。
  5. 由于运行环境的差异，在 iOS 设备上小程序内的 wxs 会比 javascript 代码快 2 ~ 20 倍。在 android 设备上二者运行效率无差异。

- json

  小程序配置，如页面注册，页面标题，tabBar，页面组件的引用。

- app.js

  小程序必须文件，在 app.js 中调用 App 方法注册小程序实例。绑定生命周期回调函数、错误监听和页面不存在监听函数等。也可以做登录验证，版本验证，全局变量等处理。

- app.json

  小程序必须文件，是小程序的配置文件入口，包括：

  - pages: 小程序页面的路径

  - window ： 小程序的状态栏，导航栏，标题，窗口背景色

  -tabbar ： 配置项指定 tab 栏的表现

  - permission : 小程序接口权限配置

  - subpackages ： 分包

> ### 微信小程序 wxml 与 html,wxss 与 css，wxs 与 js 的区别

- WXML 与 html 的区别

  1. wxml 相比于 html 使用更少的标签，常见的有 view，text,button 等

  2. 在 wxml 中可以写 wx:if.{{}},wx:for 等一些表达式

- wxss 与 css 区别

  1. wxss 新增尺寸单位 rpx

  2. wxss 仅支持部分选择器

  3. wxss 支持全局样式和局部样式

-wxs 与 js 的区别

    1. wxs可以在wxml中直接在<wxs></wxs>下编写

    2. wxs 中不能调用其他 javascript 文件中定义的函数，也不能调用小程序提供的API。

    3. 由于运行环境的差异，在 iOS 设备上小程序内的 wxs 会比 javascript 代码快 2 ~ 20 倍。在 android 设备上二者运行效率无差异。

wxs 因为运行环境不同回避 js 快，但是不能调用 js 方法和晓晨新股 api，所以一般用来在数据的处理，和一些格式化，效果的方法。

> ### 微信小程序原理

微信小程序采用 JavaScript、WXML、WXSS 三种技术进行开发,本质就是一个单页面应用，所有的页面渲染和事件处理，都在一个页面内进行，但又可以通过微信客户端调用原生的各种接口
微信的架构，是数据驱动的架构模式，它的 UI 和数据是分离的，所有的页面更新，都需要通过对数据的更改来实现

小程序分为两个部分 webview 和 appService 。其中 webview 主要用来展现 UI ，appService 有来处理业务逻辑、数据及接口调用。它们在两个进程中运行，通过系统层 JSBridge 实现通信，实现 UI 的渲染、事件的处理

> ### 微信小程序双向绑定与 Vue 有哪些不同

微信小程序改变 this.data 时不能同步的改变视图层，需要嗲用 this.setData()

> ### 如何实现下拉刷新,上拉刷新

onPullDownRefresh()
监听用户下拉刷新事件。

- 需要在 app.json 的 window 选项中或页面配置中开启 enablePullDownRefresh。
- 可以通过 wx.startPullDownRefresh 触发下拉刷新，调用后触发下拉刷新动画，效果与用户手动下拉刷新一致。
- 当处理完数据刷新后，wx.stopPullDownRefresh 可以停止当前页面的下拉刷新。

onReachBottom()
监听用户上拉触底事件。

- 可以在 app.json 的 window 选项中或页面配置中设置触发距离 onReachBottomDistance。
- 在触发距离内滑动期间，本事件只会被触发一次。

> ### 如何获取用户信息

- 获取用户基本信息

  使用 open-data 获取用户头像，昵称，群名称，性别，所在城市，用户所在省份，用户所在国家，用户的语言

  使用 wx.getUserInfo

  使用前提是用户授权，使用 wx.getSetting 授权或者使用 wx.login 登录

- 获取用户 openid

  - 方法一

  1. 首先调用 wx.login({})获取登录凭证（code）
  2. 调用接口 用拿到的 code 换取 openid

  - 方法二

  使用云函数调用

  ```
  exports.main = (event, context) => {
  // 这里获取到的 openId、 appId 和 unionId 是可信的，注意 unionId 仅在满足 unionId 获取条件时返回
  let { OPENID, APPID, UNIONID } = cloud.getWXContext()

  return {
      OPENID,
      APPID,
      UNIONID,
  }
  ```

- 获取手机号

  需要将 button 组件 open-type 的值设置为 getPhoneNumber，当用户点击并同意之后，可以通过 bindgetphonenumber 事件回调获取到微信服务器返回的加密数据， 然后在第三方服务端结合 session_key 以及 app_id 进行解密获取手机号。

> ### 用户授权

1. 需要对用户进行权限校验，判断用户是否已经对小程序进行过权限设置，使用 wx.getSetting()方法

2. 如果第一次登录，进行 wx.getSetting()进行校验，会返回 onFail 回调，这时我们需要弹出授权面板，让用户手动授权。使用<button open-type="getUserInfo" @getUserInfo="getUserINfo"></button>

3. wx.getUserInfo()获取用户信息,该函数调用成功后顾名思义会返回用户的信息，包括头像、名称等.

4. 如果在 storage 中找不到 openId，则会进行调用 wx.login()方法

我们需要先使用 wx.getSettings 来进行授权的判断，如果判断成功，获得了授权，就可以执行业务的代码。如果判断不成功，需要则使用 wx.authorize 来进行授权的获取。同样的，获取成功后就可以执行业务代码。如果获取失败，则需要重新获取授权。这时，你可以使用 wx.openSettngs 来打开配置界面，让用户主动配置，开启授权。

> ### 如何获取用户授权，用户如果取消授权后如何再次授权

- 判断用户是否授权，使用 wx.getSetting()，如果判断不成功，需要则使用 wx.authorize 来进行授权的获取。在授权失败时，我们调用 wx.showModel()方法。跳转去设置页。设置页只有<button open-type="getUserInfo">.用户去系统设置页设置后就可以拿到信息。

> ### 如何获取元素信息

```
 getRect () {
    wx.createSelectorQuery().select('#the-id').boundingClientRect(function(rect){
      rect.id      // 节点的ID
      rect.dataset // 节点的dataset
      rect.left    // 节点的左边界坐标
      rect.right   // 节点的右边界坐标
      rect.top     // 节点的上边界坐标
      rect.bottom  // 节点的下边界坐标
      rect.width   // 节点的宽度
      rect.height  // 节点的高度
    }).exec()
  },
  getAllRects () {
    wx.createSelectorQuery().selectAll('.a-class').boundingClientRect(function(rects){
      rects.forEach(function(rect){
        rect.id      // 节点的ID
        rect.dataset // 节点的dataset
        rect.left    // 节点的左边界坐标
        rect.right   // 节点的右边界坐标
        rect.top     // 节点的上边界坐标
        rect.bottom  // 节点的下边界坐标
        rect.width   // 节点的宽度
        rect.height  // 节点的高度
      })
    }).exec()
  }
```

> ### 微信小程序传参方式有哪些，组件传参方式

- 小程序页面传参方式

  1. navigator 地址传参

  ```
  wx.navagatorTo({
    url : '../index/index?id=xxx&name=xxx'
  })
  ```

  去值在 onLoad 中参数获取

  2. 全局变量
     app.js 页面

  ```
  globalData:{
    id:2
  }
  ```

  取值

  ```
  var id = app.globalData.id
  ```

  3.  绑定事件通过 data-传参，取值在方法中 e.currentTarget.dataset 中取值

  4.  form 表单传值

  5. Storage传值

- 小程序组件传参方法

  1. 父子组件传参

  ```
  //父组件引用子组件，直接在标签内传值
  <child id="1"></child>
  //子组件在properties中接收
  properties : {
    id : String
  }
  ```

  2. 子组件向父组件传参

  ```
  //父组件中绑定一个事件用来接收参数
  <child bind:myevent="OnMyEvent"></child>
  methods: {
    onMyEvent:function(e){
      this.setData({
        paramBtoA: e.detail.id
      })
    }
  }
  //子组件中在methods中写方法
  change:function(){
    this.triggerEvent('myevent', { id:123});
  }
  ```

  3. 兄弟组件传参

    通过父组件进行传递，子组件A传值到父组件，父组件setData值，再将值传递给组件B

> ### 小程序生命周期，小程序页面生命周期，小程序组件生命周期


- 小程序生命周期

  - onLaunch      监听小程序初始化。

  - onShow        监听小程序启动或切前台。

  - onHide        监听小程序切后台。

  - onError       错误监听函数。

- 小程序页面生命周期

  - onLoad 监听页面加载

  - onShow 加载完成后、后台切到前台或重新进入页面时触发

  - onReady 页面首次渲染完成时触发

  - onHide 从前台切到后台或进入其他页面触发

  - 页面卸载时触发

- 组件生命周期 （在lifetimes定义）

  - created 在组件实例刚刚被创建时执行，注意此时不能调用 setData 

  - attached 在组件实例进入页面节点树时执行

  - ready 在组件布局完成后执行

  - moved 在组件实例被移动到节点树另一个位置时执行

  - detached 在组件实例被从页面节点树移除时执行

  - error   每当组件方法抛出错误时执行

- 组件所在页面的生命周期

  - show  组件所在的页面被展示时执行

  - hide 组件所在的页面被隐藏时执行

  - resize 组件所在的页面尺寸变化时执行 

> ### 哪些方法可以用来提高微信小程序的应用速度

  - 分包处理，使主包体积变小，减小加载包时的白屏时间

  - 图片压缩，减少接口请求

  - 缩减代码体谅，重复代码使用组件

  - 减少setData的使用 

  - 数据量过大的时候考虑分页，上拉加载

  - 将常用数据进行缓存，避免重复异步请求

  - 避免不当使用onPageScroll

> ### 微信小程序有哪些跳转方式，有什么区别

  -  wx.switchTab  跳转到 tabBar 页面，并关闭其他所有非 tabBar 页面

  - wx.reLaunch 关闭所有页面，打开到应用内的某个页面

  - wx.redirectTo 关闭当前页面，跳转到应用内的某个页面。但是不允许跳转到 tabbar 页面。

  - wx.navigateTo 保留当前页面，跳转到应用内的某个页面。但是不能跳到 tabbar 页面。  小程序中页面栈最多十层

  - wx.navigateBack 关闭当前页面，返回上一页面或多级页面。

> ### 微信小程序长按识别二维码

  - image show-menu-by-longpress 属性

  - 使用wx.previewImage()方法

  ```
    wx.previewImage({
      current: current,
      urls: [current]
    })

  ``` 

> ### 代码审核和发布

- 审核

  - 进入微信公众平台，点击开发管理

  - 在小程序开发者上传的开发版本点击提交审核

  - 点击【提交审核】，确认协议后，在新打开的页面中填写资料。

- 发布

 - 进入微信公众平台，点击开发管理

 - 在右侧的的【审核版本】中找到【审核通过，待发布】的版本。

 - 点击【提交发布】按钮，扫码确认后即可发布你的小程序。

> ### 小程序申请微信支付

````
    wx.requestPayment({
      timeStamp : '', // 时间戳，必填（后台传回）
      nonceStr : '', // 随机字符串，必填（后台传回）
      package : '', // 统一下单接口返回的 prepay_id 参数值，必填（后台传回）
      signType : 'MD5', // 签名算法，非必填，（预先约定或者后台传回）
      paySign  : '', // 签名 ，必填 （后台传回）
      success:function(res){ // 成功后的回调函数
          // do something
    }
})
````

> ### 转发分享

- 获取转发信息

现在通过调用 wx.showShareMenu 并且设置 withShareTicket 为 true ，当用户将小程序转发到任一群聊之后，此转发卡片在群聊中被其他用户打开时，可以在 App.onLaunch 或 App.onShow 获取到一个 shareTicket。通过调用 wx.getShareInfo 接口传入此 shareTicket 可以获取到转发信息。

- 页面内发起转发

 通过给 button 组件设置属性 open-type="share"，可以在用户点击按钮后触发 Page.onShareAppMessage 事件

- Tips

  - 不自定义转发图片的情况下，默认会取当前页面，从顶部开始，高度为 80% 屏幕宽度的图像作为转发图片。

  - 转发的调试支持请查看 普通转发的调试支持 和 带 shareTicket 的转发

  - 只有转发到群聊中打开才可以获取到 shareTickets 返回值，单聊没有 shareTickets

  - shareTicket 仅在当前小程序生命周期内有效

  - 由于策略变动，小程序群相关能力进行调整，开发者可先使用 wx.getShareInfo 接口中的群 ID 进行功能开发。


> ### 微信小程序的优劣势

- 优势

  - 无需下载，通过搜索和扫一扫就可以打开。

  - 开发成本要比App要低。

  - 安依托微信流量，天生推广传播优势

  - 为用户提供良好的安全保障。小程序的发布，微信拥有一套严格的审查流程， 不能通过审查的小程序是无法发布到线上的。

- 劣势

  - 限制较多,页面大小不能超过2M。不能打开超过10个层级的页面

  - 一些功能需要审核，限制了功能

> ### 小程序获取地理位置


```
wx.getSetting({
      success: (res) => {
        console.log(JSON.stringify(res))
        // res.authSetting['scope.userLocation'] == undefined    表示 初始化进入该页面
        // res.authSetting['scope.userLocation'] == false    表示 非初始化进入该页面,且未授权
        // res.authSetting['scope.userLocation'] == true    表示 地理位置授权
        if (res.authSetting['scope.userLocation'] != undefined && res.authSetting['scope.userLocation'] != true) {
          wx.showModal({
            title: '请求授权当前位置',
            content: '需要获取您的地理位置，请确认授权',
            success: function (res) {
              if (res.cancel) {
                wx.showToast({
                  title: '拒绝授权',
                  icon: 'none',
                  duration: 1000
                })
              } else if (res.confirm) {
                wx.openSetting({
                  success: function (dataAu) {
                    if (dataAu.authSetting["scope.userLocation"] == true) {
                      wx.showToast({
                        title: '授权成功',
                        icon: 'success',
                        duration: 1000
                      })
                      //再次授权，调用wx.getLocation的API
                      
                    } else {
                      wx.showToast({
                        title: '授权失败',
                        icon: 'none',
                        duration: 1000
                      })
                    }
                  }
                })
              }
            }
          })
        } else if (res.authSetting['scope.userLocation'] == undefined) {
          //调用wx.getLocation的API
        }
        else {
          //调用wx.getLocation的API
        }
      }
    })
```
在拿到用户授权以后，使用微信的API获取当前位置的经纬度微信获取位置API
```
onLoad: function () {
      wx.getLocation({
        success: res=> {
          console.log(res);
          this.setData({
            location: res,
          })
          // console.log(app.globalData.location);
        },
      })
}
```
> ### 小程序冷启动和热启动

热启动：假如用户已经打开过某小程序，然后在一定时间内再次打开该小程序，此时无需重新启动，只需将后台态的小程序切换到前台，这个过程就是热启动；
冷启动：用户首次打开或小程序被微信主动销毁后再次打开的情况，此时小程序需要重新加载启动，即冷启动。
小程序冷启动时，如果发现有新版本，将会异步下载新版本的代码包，并同时用客户端本地的包进行启动，即新版本的小程序需要等下一次冷启动才会应用上。
```
const updateManager = wx.getUpdateManager()

updateManager.onCheckForUpdate(function (res) {
  // 请求完新版本信息的回调
  console.log(res.hasUpdate)
})

updateManager.onUpdateReady(function () {
  wx.showModal({
    title: '更新提示',
    content: '新版本已经准备好，是否重启应用？',
    success(res) {
      if (res.confirm) {
        // 新的版本已经下载好，调用 applyUpdate 应用新版本并重启
        updateManager.applyUpdate()
      }
    }
  })
})

updateManager.onUpdateFailed(function () {
  // 新版本下载失败
})
```

<!-- > ### 小程序登录流程 -->



<!-- > ### 使用 webview 直接加载要注意哪些事项 -->

<!-- > ### 小程序云开发 -->