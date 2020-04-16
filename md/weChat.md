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

微信小程序改变this.data时不能同步的改变视图层，需要嗲用this.setData()

> ### 如何实现下拉刷新,上拉刷新

onPullDownRefresh()
监听用户下拉刷新事件。

- 需要在app.json的window选项中或页面配置中开启enablePullDownRefresh。
- 可以通过wx.startPullDownRefresh触发下拉刷新，调用后触发下拉刷新动画，效果与用户手动下拉刷新一致。
- 当处理完数据刷新后，wx.stopPullDownRefresh可以停止当前页面的下拉刷新。

onReachBottom()
监听用户上拉触底事件。

- 可以在app.json的window选项中或页面配置中设置触发距离onReachBottomDistance。
- 在触发距离内滑动期间，本事件只会被触发一次。

> ### 如何获取用户信息

- 获取用户基本信息

    使用open-data获取用户头像，昵称，群名称，性别，所在城市，用户所在省份，用户所在国家，用户的语言

    使用wx.getUserInfo

    使用前提是用户授权，使用 wx.getSetting授权或者使用wx.login登录

- 获取用户openid

    - 方法一

    1. 首先调用wx.login({})获取登录凭证（code）
    2. 调用接口 用拿到的code 换取 openid

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

> ### 如何获取用户授权，用户如果取消授权后如何再次授权

> ### 如何获取元素信息

> ### 微信小程序传参方式有哪些，组件传参方式

> ### 小程序生命周期和小程序页面生命周期

> ### 使用 webview 直接加载要注意哪些事项

> ### 哪些方法可以用来提高微信小程序的应用速度

> ### 微信小程序有哪些跳转方式，有什么区别

> ### 微信小程序长按识别二维码

> ### 微信小程序获取用户信息

> ### 代码审核和发布

> ### 小程序申请微信支付

> ### 转发分享

> ### 微信小程序的优劣势

> ### 小程序解析富文本编辑器

> ### 小程序获取地理位置

> ### 小程序云开发

> ### 小程序登录流程
