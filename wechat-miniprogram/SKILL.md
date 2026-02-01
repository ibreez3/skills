---
name: wechat-miniprogram
description: WeChat Mini Program development guide. Create, build, and optimize WeChat Mini Programs including WXML, WXSS, JavaScript/TypeScript, components, APIs, state management, and best practices. Use this skill when developing WeChat Mini Programs.
license: MIT
---

This skill provides comprehensive guidance for WeChat Mini Program (微信小程序) development, from project setup to deployment and optimization.

The user invokes this skill by asking to create, develop, fix, or optimize WeChat Mini Program code.

## Project Structure

### Standard Directory Layout

```
miniprogram/
├── pages/                    # 页面目录
│   ├── index/               # 首页
│   │   ├── index.js         # 页面逻辑
│   │   ├── index.json       # 页面配置
│   │   ├── index.wxml       # 页面结构
│   │   └── index.wxss       # 页面样式
│   ├── user/
│   │   ├── user.js
│   │   ├── user.json
│   │   ├── user.wxml
│   │   └── user.wxss
│   └── ...
├── components/              # 自定义组件
│   ├── custom-button/
│   │   ├── custom-button.js
│   │   ├── custom-button.json
│   │   ├── custom-button.wxml
│   │   └── custom-button.wxss
│   └── ...
├── utils/                   # 工具函数
│   ├── util.js
│   ├── request.js          # 网络请求封装
│   └── storage.js          # 本地存储封装
├── services/               # API 服务
│   ├── api.js
│   └── auth.js
├── store/                  # 状态管理
│   └── store.js
├── images/                 # 图片资源
├── styles/                 # 全局样式
│   └── common.wxss
├── app.js                  # 小程序逻辑
├── app.json                # 小程序配置
├── app.wxss                # 小程序全局样式
├── sitemap.json            # 搜索配置
└── project.config.json     # 项目配置
```

### Configuration Files

**app.json - Global Configuration**

```json
{
  "pages": [
    "pages/index/index",
    "pages/user/user"
  ],
  "window": {
    "navigationBarTitleText": "小程序名称",
    "navigationBarBackgroundColor": "#ffffff",
    "navigationBarTextStyle": "black",
    "backgroundColor": "#f8f8f8",
    "backgroundTextStyle": "light",
    "enablePullDownRefresh": false
  },
  "tabBar": {
    "color": "#999999",
    "selectedColor": "#1AAD19",
    "backgroundColor": "#ffffff",
    "borderStyle": "black",
    "list": [
      {
        "pagePath": "pages/index/index",
        "text": "首页",
        "iconPath": "images/home.png",
        "selectedIconPath": "images/home-active.png"
      },
      {
        "pagePath": "pages/user/user",
        "text": "我的",
        "iconPath": "images/user.png",
        "selectedIconPath": "images/user-active.png"
      }
    ]
  },
  "networkTimeout": {
    "request": 10000,
    "downloadFile": 10000
  },
  "debug": false,
  "sitemapLocation": "sitemap.json",
  "style": "v2",
  "lazyCodeLoading": "requiredComponents"
}
```

**project.config.json - Project Configuration**

```json
{
  "description": "项目配置文件",
  "packOptions": {
    "ignore": [],
    "include": []
  },
  "setting": {
    "urlCheck": true,
    "es6": true,
    "enhance": true,
    "postcss": true,
    "preloadBackgroundData": false,
    "minified": true,
    "newFeature": false,
    "coverView": true,
    "nodeModules": false,
    "autoAudits": false,
    "showShadowRootInWxmlPanel": true,
    "scopeDataCheck": false,
    "uglifyFileName": false,
    "checkInvalidKey": true,
    "checkSiteMap": true,
    "uploadWithSourceMap": true,
    "compileHotReLoad": true,
    "lazyloadPlaceholderEnable": false,
    "useMultiFrameRuntime": true,
    "useApiHook": true,
    "useApiHostProcess": true,
    "babelSetting": {
      "ignore": [],
      "disablePlugins": [],
      "outputPath": ""
    },
    "useIsolateContext": true,
    "userConfirmedBundleSwitch": false,
    "packNpmManually": false,
    "packNpmRelationList": [],
    "minifyWXSS": true,
    "disableUseStrict": false,
    "minifyWXML": true,
    "showES6CompileOption": false,
    "useCompilerPlugins": false
  },
  "compileType": "miniprogram",
  "libVersion": "3.4.7",
  "appid": "your_appid",
  "projectname": "project_name",
  "condition": {}
}
```

## Core Development

### Page Development

**index.js - Page Logic**

```javascript
// 获取应用实例
const app = getApp()

Page({
  // 页面数据
  data: {
    motto: 'Hello World',
    userInfo: {},
    hasUserInfo: false,
    canIUse: wx.canIUse('button.open-type.getUserInfo'),
    list: []
  },

  // 生命周期函数
  onLoad(options) {
    console.log('Page onLoad', options)

    // 页面加载时获取数据
    this.loadData()

    // 获取用户信息
    if (app.globalData.userInfo) {
      this.setData({
        userInfo: app.globalData.userInfo,
        hasUserInfo: true
      })
    } else if (this.data.canIUse) {
      // 由于 getUserInfo 是网络请求，可能会在 Page.onLoad 之后才返回
      app.userInfoReadyCallback = res => {
        this.setData({
          userInfo: res.userInfo,
          hasUserInfo: true
        })
      }
    }
  },

  onReady() {
    // 页面初次渲染完成
    console.log('Page onReady')
  },

  onShow() {
    // 页面显示
    console.log('Page onShow')
  },

  onHide() {
    // 页面隐藏
    console.log('Page onHide')
  },

  onUnload() {
    // 页面卸载
    console.log('Page onUnload')
  },

  onPullDownRefresh() {
    // 下拉刷新
    this.loadData().then(() => {
      wx.stopPullDownRefresh()
    })
  },

  onReachBottom() {
    // 上拉触底
    this.loadMore()
  },

  onShareAppMessage() {
    // 分享
    return {
      title: '自定义分享标题',
      path: '/pages/index/index',
      imageUrl: ''
    }
  },

  // 自定义方法
  loadData() {
    // 显示加载提示
    wx.showLoading({
      title: '加载中...',
      mask: true
    })

    return api.getDataList()
      .then(res => {
        this.setData({
          list: res.data
        })
        wx.hideLoading()
      })
      .catch(err => {
        wx.hideLoading()
        wx.showToast({
          title: '加载失败',
          icon: 'none'
        })
      })
  },

  loadMore() {
    // 加载更多
    console.log('Load more data')
  },

  // 事件处理
  handleTap(e) {
    console.log('Button tapped', e)
    const { id } = e.currentTarget.dataset
    wx.navigateTo({
      url: `/pages/detail/detail?id=${id}`
    })
  },

  getUserInfo(e) {
    console.log(e)
    app.globalData.userInfo = e.detail.userInfo
    this.setData({
      userInfo: e.detail.userInfo,
      hasUserInfo: true
    })
  }
})
```

**index.wxml - Page Structure**

```xml
<!--index.wxml-->
<view class="container">
  <!-- 用户信息 -->
  <view class="userinfo">
    <button
      wx:if="{{!hasUserInfo && canIUse}}"
      open-type="getUserInfo"
      bindgetuserinfo="getUserInfo">
      获取头像昵称
    </button>
    <block wx:else>
      <image bindtap="bindViewTap" class="userinfo-avatar" src="{{userInfo.avatarUrl}}" mode="cover"></image>
      <text class="userinfo-nickname">{{userInfo.nickName}}</text>
    </block>
  </view>

  <!-- 列表 -->
  <view class="usermotto">
    <text class="user-motto">{{motto}}</text>
  </view>

  <!-- 列表渲染 -->
  <view class="list">
    <block wx:for="{{list}}" wx:key="id">
      <view
        class="list-item"
        data-id="{{item.id}}"
        bindtap="handleTap">
        <image class="item-image" src="{{item.image}}" />
        <view class="item-info">
          <text class="item-title">{{item.title}}</text>
          <text class="item-desc">{{item.description}}</text>
        </view>
      </view>
    </block>
  </view>

  <!-- 条件渲染 -->
  <view wx:if="{{list.length === 0}}" class="empty">
    <text>暂无数据</text>
  </view>
</view>
```

**index.wxss - Page Styles**

```css
/* index.wxss */
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20rpx;
  box-sizing: border-box;
}

.userinfo {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 40rpx 0;
}

.userinfo-avatar {
  width: 128rpx;
  height: 128rpx;
  border-radius: 50%;
}

.userinfo-nickname {
  margin-top: 20rpx;
  color: #333;
}

.list {
  width: 100%;
  margin-top: 40rpx;
}

.list-item {
  display: flex;
  padding: 20rpx;
  margin-bottom: 20rpx;
  background: #fff;
  border-radius: 10rpx;
  box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.1);
}

.item-image {
  width: 120rpx;
  height: 120rpx;
  border-radius: 8rpx;
  flex-shrink: 0;
}

.item-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-left: 20rpx;
}

.item-title {
  font-size: 32rpx;
  color: #333;
  font-weight: 500;
}

.item-desc {
  font-size: 26rpx;
  color: #999;
}

.empty {
  padding: 100rpx 0;
  text-align: center;
  color: #999;
}
```

### Custom Component

**custom-button.js**

```javascript
Component({
  // 组件属性
  properties: {
    text: {
      type: String,
      value: '按钮'
    },
    type: {
      type: String,
      value: 'default'  // default, primary, warn
    },
    disabled: {
      type: Boolean,
      value: false
    },
    size: {
      type: String,
      value: 'medium'  // small, medium, large
    }
  },

  // 组件数据
  data: {
    buttonClass: ''
  },

  // 生命周期
  lifetimes: {
    attached() {
      this._initButtonClass()
    }
  },

  // 监听属性变化
  observers: {
    'type, disabled': function(type, disabled) {
      this._initButtonClass()
    }
  },

  // 组件方法
  methods: {
    _initButtonClass() {
      const { type, disabled, size } = this.properties
      let classList = ['custom-button']

      classList.push(`btn-${type}`)
      if (disabled) {
        classList.push('btn-disabled')
      }
      classList.push(`btn-${size}`)

      this.setData({ buttonClass: classList.join(' ') })
    },

    handleClick(e) {
      if (this.properties.disabled) return

      this.triggerEvent('click', {
        event: e,
        data: this.properties
      })
    }
  }
})
```

**custom-button.wxml**

```xml
<!--custom-button.wxml-->
<button
  class="{{buttonClass}}"
  disabled="{{disabled}}"
  bindtap="handleClick">
  <slot></slot>
  <text wx:if="{{!slots.default}}">{{text}}</text>
</button>
```

**custom-button.wxss**

```css
/* custom-button.wxss */
.custom-button {
  border: none;
  border-radius: 8rpx;
  transition: all 0.3s;
}

.custom-button.btn-small {
  font-size: 24rpx;
  padding: 10rpx 30rpx;
}

.custom-button.btn-medium {
  font-size: 28rpx;
  padding: 15rpx 40rpx;
}

.custom-button.btn-large {
  font-size: 32rpx;
  padding: 20rpx 50rpx;
}

.custom-button.btn-default {
  background-color: #f0f0f0;
  color: #333;
}

.custom-button.btn-primary {
  background-color: #1AAD19;
  color: #fff;
}

.custom-button.btn-warn {
  background-color: #E64340;
  color: #fff;
}

.custom-button.btn-disabled {
  opacity: 0.6;
}
```

**custom-button.json**

```json
{
  "component": true,
  "usingComponents": {}
}
```

## API Integration

### Request Encapsulation

**utils/request.js**

```javascript
// 基础 URL 配置
const BASE_URL = 'https://api.example.com'

// 请求拦截器
function request(options) {
  const {
    url,
    method = 'GET',
    data = {},
    header = {},
    showLoading = true,
    loadingText = '加载中...'
  } = options

  // 显示 loading
  if (showLoading) {
    wx.showLoading({
      title: loadingText,
      mask: true
    })
  }

  // 获取 token
  const token = wx.getStorageSync('token') || ''

  return new Promise((resolve, reject) => {
    wx.request({
      url: BASE_URL + url,
      method,
      data,
      header: {
        'Content-Type': 'application/json',
        'Authorization': token ? `Bearer ${token}` : '',
        ...header
      },
      success: (res) => {
        // 隐藏 loading
        if (showLoading) {
          wx.hideLoading()
        }

        // 处理响应
        if (res.statusCode === 200) {
          const { code, data, message } = res.data

          // 业务成功
          if (code === 0 || code === 200) {
            resolve(data)
          } else {
            // 业务失败
            handleError(code, message)
            reject(res.data)
          }
        } else if (res.statusCode === 401) {
          // token 过期，重新登录
          handleUnauthorized()
          reject(res)
        } else {
          // HTTP 错误
          wx.showToast({
            title: '网络请求失败',
            icon: 'none'
          })
          reject(res)
        }
      },
      fail: (err) => {
        if (showLoading) {
          wx.hideLoading()
        }

        wx.showToast({
          title: '网络连接失败',
          icon: 'none'
        })
        reject(err)
      }
    })
  })
}

// 错误处理
function handleError(code, message) {
  const errorMap = {
    400: '请求参数错误',
    403: '没有权限',
    404: '资源不存在',
    500: '服务器错误'
  }

  wx.showToast({
    title: message || errorMap[code] || '请求失败',
    icon: 'none'
  })
}

// 处理未授权
function handleUnauthorized() {
  // 清除 token
  wx.removeStorageSync('token')

  // 跳转到登录页
  wx.reLaunch({
    url: '/pages/login/login'
  })
}

// 封装常用方法
const http = {
  get(url, data, options = {}) {
    return request({
      url,
      method: 'GET',
      data,
      ...options
    })
  },

  post(url, data, options = {}) {
    return request({
      url,
      method: 'POST',
      data,
      ...options
    })
  },

  put(url, data, options = {}) {
    return request({
      url,
      method: 'PUT',
      data,
      ...options
    })
  },

  delete(url, data, options = {}) {
    return request({
      url,
      method: 'DELETE',
      data,
      ...options
    })
  },

  upload(url, filePath, options = {}) {
    const token = wx.getStorageSync('token') || ''

    return new Promise((resolve, reject) => {
      wx.uploadFile({
        url: BASE_URL + url,
        filePath,
        name: 'file',
        header: {
          'Authorization': token ? `Bearer ${token}` : ''
        },
        success: (res) => {
          const data = JSON.parse(res.data)
          if (data.code === 0) {
            resolve(data.data)
          } else {
            reject(data)
          }
        },
        fail: reject
      })
    })
  }
}

module.exports = http
```

### Service Layer

**services/api.js**

```javascript
const http = require('../utils/request')

// 用户相关
const userApi = {
  // 登录
  login(code) {
    return http.post('/user/login', { code })
  },

  // 获取用户信息
  getUserInfo() {
    return http.get('/user/info')
  },

  // 更新用户信息
  updateUserInfo(data) {
    return http.put('/user/info', data)
  }
}

// 数据相关
const dataApi = {
  // 获取列表
  getList(page = 1, pageSize = 10) {
    return http.get('/data/list', { page, pageSize })
  },

  // 获取详情
  getDetail(id) {
    return http.get(`/data/${id}`)
  },

  // 创建
  create(data) {
    return http.post('/data', data)
  },

  // 更新
  update(id, data) {
    return http.put(`/data/${id}`, data)
  },

  // 删除
  delete(id) {
    return http.delete(`/data/${id}`)
  }
}

module.exports = {
  userApi,
  dataApi
}
```

## State Management

### Simple Store Pattern

**store/store.js**

```javascript
class Store {
  constructor() {
    this.state = {
      userInfo: null,
      token: null,
      cartList: []
    }
    this.listeners = []
  }

  // 获取状态
  getState() {
    return this.state
  }

  // 更新状态
  setState(newState) {
    this.state = { ...this.state, ...newState }

    // 持久化到本地
    this._persist()

    // 通知订阅者
    this._notify()
  }

  // 订阅状态变化
  subscribe(listener) {
    this.listeners.push(listener)

    // 返回取消订阅函数
    return () => {
      const index = this.listeners.indexOf(listener)
      this.listeners.splice(index, 1)
    }
  }

  // 通知订阅者
  _notify() {
    this.listeners.forEach(listener => listener(this.state))
  }

  // 持久化到本地
  _persist() {
    const { userInfo, token } = this.state
    if (token) {
      wx.setStorageSync('token', token)
    }
    if (userInfo) {
      wx.setStorageSync('userInfo', userInfo)
    }
  }

  // 从本地恢复
  _restore() {
    const token = wx.getStorageSync('token')
    const userInfo = wx.getStorageSync('userInfo')

    if (token || userInfo) {
      this.state = {
        ...this.state,
        token,
        userInfo
      }
    }
  }

  // Actions
  login(userInfo, token) {
    this.setState({
      userInfo,
      token
    })
  }

  logout() {
    this.setState({
      userInfo: null,
      token: null
    })

    // 清除本地存储
    wx.removeStorageSync('token')
    wx.removeStorageSync('userInfo')
  }

  addToCart(item) {
    const { cartList } = this.state
    const exists = cartList.find(i => i.id === item.id)

    if (exists) {
      exists.quantity += item.quantity || 1
    } else {
      cartList.push({ ...item, quantity: item.quantity || 1 })
    }

    this.setState({ cartList })
  }

  removeFromCart(id) {
    const cartList = this.state.cartList.filter(item => item.id !== id)
    this.setState({ cartList })
  }
}

// 创建单例
const store = new Store()
store._restore()

module.exports = store
```

### Using Store in Page

```javascript
const store = require('../../store/store')

Page({
  data: {
    userInfo: null
  },

  onLoad() {
    // 获取初始状态
    const state = store.getState()
    this.setData({
      userInfo: state.userInfo
    })

    // 订阅状态变化
    this.unsubscribe = store.subscribe((state) => {
      this.setData({
        userInfo: state.userInfo
      })
    })
  },

  onUnload() {
    // 取消订阅
    if (this.unsubscribe) {
      this.unsubscribe()
    }
  },

  handleLogin() {
    // 登录成功后更新状态
    store.login(userInfo, token)
  }
})
```

## Common APIs

### Storage

```javascript
// 同步存储
wx.setStorageSync('key', value)
const value = wx.getStorageSync('key')
wx.removeStorageSync('key')
wx.clearStorageSync()

// 异步存储
wx.setStorage({
  key: 'key',
  data: value
})

wx.getStorage({
  key: 'key',
  success(res) {
    console.log(res.data)
  }
})

wx.removeStorage({
  key: 'key'
})

wx.clearStorage()

// 获取存储信息
wx.getStorageInfo({
  success(res) {
    console.log(res.keys)
    console.log(res.currentSize)
    console.log(res.limitSize)
  }
})
```

### Navigation

```javascript
// 保留当前页面，跳转到应用内的某个页面
wx.navigateTo({
  url: '/pages/detail/detail?id=1&name=test'
})

// 关闭当前页面，跳转到应用内的某个页面
wx.redirectTo({
  url: '/pages/login/login'
})

// 跳转到 tabBar 页面
wx.switchTab({
  url: '/pages/index/index'
})

// 关闭所有页面，打开到应用内的某个页面
wx.reLaunch({
  url: '/pages/index/index'
})

// 返回上一页面或多级页面
wx.navigateBack({
  delta: 1
})

// 获取页面参数
Page({
  onLoad(options) {
    console.log(options.id)    // 1
    console.log(options.name)  // test
  }
})
```

### Toast & Modal

```javascript
// 提示框
wx.showToast({
  title: '成功',
  icon: 'success',
  duration: 2000
})

// 加载提示
wx.showLoading({
  title: '加载中...',
  mask: true
})

wx.hideLoading()

// 模态对话框
wx.showModal({
  title: '提示',
  content: '确定要删除吗？',
  success(res) {
    if (res.confirm) {
      console.log('用户点击确定')
    } else if (res.cancel) {
      console.log('用户点击取消')
    }
  }
})

// 操作菜单
wx.showActionSheet({
  itemList: ['拍照', '从相册选择'],
  success(res) {
    console.log(res.tapIndex)
  }
})
```

### Login & Authorization

```javascript
Page({
  // 微信登录
  wxLogin() {
    wx.login({
      success: (res) => {
        if (res.code) {
          // 发送 code 到服务器
          api.login(res.code).then(result => {
            const { token, userInfo } = result

            // 存储token
            wx.setStorageSync('token', token)

            // 更新状态
            store.login(userInfo, token)
          })
        } else {
          console.error('登录失败', res.errMsg)
        }
      }
    })
  },

  // 获取用户信息（按钮授权）
  getUserInfo(e) {
    if (e.detail.userInfo) {
      // 用户同意授权
      const { userInfo } = e.detail
      this.setData({ userInfo })

      // 上传用户信息到服务器
      api.updateUserInfo(userInfo)
    } else {
      // 用户拒绝授权
      wx.showToast({
        title: '需要授权才能使用',
        icon: 'none'
      })
    }
  },

  // 获取手机号
  getPhoneNumber(e) {
    if (e.detail.encryptedData && e.detail.iv) {
      // 发送到服务器解密
      api.getPhoneNumber({
        encryptedData: e.detail.encryptedData,
        iv: e.detail.iv
      }).then(result => {
        console.log('手机号:', result.phoneNumber)
      })
    }
  }
})
```

### Payment

```javascript
Page({
  // 发起支付
  pay(orderId) {
    // 先调用服务器获取支付参数
    api.createPayment(orderId).then(paymentParams => {
      wx.requestPayment({
        timeStamp: paymentParams.timeStamp,
        nonceStr: paymentParams.nonceStr,
        package: paymentParams.package,
        signType: 'MD5',
        paySign: paymentParams.paySign,
        success: () => {
          wx.showToast({
            title: '支付成功'
          })
          // 跳转到订单详情
          wx.navigateTo({
            url: `/pages/order/detail?id=${orderId}`
          })
        },
        fail: (err) => {
          if (err.errMsg === 'requestPayment:fail cancel') {
            wx.showToast({
              title: '已取消支付',
              icon: 'none'
            })
          } else {
            wx.showToast({
              title: '支付失败',
              icon: 'none'
            })
          }
        }
      })
    })
  }
})
```

## Performance Optimization

### Best Practices

```javascript
// 1. 避免频繁调用 setData
Page({
  // ❌ 不好的做法
  updateData() {
    this.setData({ name: 'John' })
    this.setData({ age: 25 })
    this.setData({ city: 'Beijing' })
  },

  // ✅ 好的做法 - 合并 setData
  updateData() {
    this.setData({
      name: 'John',
      age: 25,
      city: 'Beijing'
    })
  },

  // 2. 只更新需要变化的数据
  updateItem(index) {
    // ❌ 更新整个数组
    this.setData({
      list: this.data.list
    })

    // ✅ 只更新特定项
    this.setData({
      [`list[${index}].name`]: 'New Name'
    })
  },

  // 3. 使用长列表优化
  data: {
    list: []
  },

  onLoad() {
    // ❌ 一次性加载所有数据
    this.loadData(1, 1000)

    // ✅ 分页加载
    this.loadData(1, 20)
  },

  onReachBottom() {
    // 上拉加载更多
    const page = this.data.page + 1
    this.loadData(page, 20)
  }
})
```

### Image Optimization

```xml
<!-- 使用懒加载 -->
<image lazy-load src="{{item.image}}" />

<!-- 指定图片模式 -->
<image mode="aspectFill" src="{{image}}" />

<!-- WebP 格式（如果支持） -->
<image src="{{item.image}}.webp" />
```

### Code Optimization

```javascript
// 1. 按需加载组件
{
  "usingComponents": {
    "custom-component": "/components/custom-component"
  },
  "lazyCodeLoading": "requiredComponents"
}

// 2. 分包加载
{
  "subPackages": [
    {
      "root": "packageA",
      "pages": [
        "pages/cat/cat",
        "pages/dog/dog"
      ]
    },
    {
      "root": "packageB",
      "pages": [
        "pages/apple/apple",
        "pages/banana/banana"
      ]
    }
  ],
  "preloadRule": {
    "pages/index/index": {
      "network": "all",
      "packages": ["packageA"]
    }
  }
}

// 3. 防抖和节流
// utils/util.js
function debounce(fn, delay = 300) {
  let timer = null
  return function(...args) {
    if (timer) clearTimeout(timer)
    timer = setTimeout(() => {
      fn.apply(this, args)
    }, delay)
  }
}

function throttle(fn, delay = 300) {
  let last = 0
  return function(...args) {
    const now = Date.now()
    if (now - last > delay) {
      last = now
      fn.apply(this, args)
    }
  }
}

module.exports = {
  debounce,
  throttle
}
```

## Debugging

### Debug Mode

```javascript
// app.json
{
  "debug": true  // 开启调试模式
}

// 在代码中使用
if (wx.getStorageSync('debug')) {
  console.log('Debug info')
}
```

### vConsole

```javascript
// 使用微信开发者工具自带的调试工具
// 在真机上可以查看日志、网络请求等
```

### Performance Monitoring

```javascript
// 性能监控
wx.getPerformance({
  success(res) {
    console.log(res)
  }
})

// 监听性能
const performance = wx.getPerformance()
const observer = performance.createObserver((entryList) => {
  entryList.forEach(entry => {
    console.log(entry.entryType, entry.name, entry.duration)
  })
})

observer.observe({ entryTypes: ['render', 'navigation'] })
```

## Deployment

### Build Process

```bash
# 1. 在微信开发者工具中点击"上传"
# 2. 填写版本号和项目备注
# 3. 上传成功后在微信公众平台提交审核
```

### Version Management

```json
// project.config.json
{
  "condition": {
    "miniprogram": {
      "list": [
        {
          "name": "开发环境",
          "pathName": "pages/index/index",
          "query": "env=dev",
          "scene": null
        }
      ]
    }
  }
}
```

### Environment Configuration

```javascript
// utils/config.js
const env = 'production'  // 或 'development', 'staging'

const config = {
  development: {
    baseUrl: 'https://dev-api.example.com',
    appId: 'dev_appid'
  },
  staging: {
    baseUrl: 'https://staging-api.example.com',
    appId: 'staging_appid'
  },
  production: {
    baseUrl: 'https://api.example.com',
    appId: 'prod_appid'
  }
}

module.exports = config[env]
```

## Common Issues & Solutions

### Common Problems

```javascript
// 1. setData 数据不更新
// 确保数据路径正确
this.setData({
  'obj.name': 'New Name'  // ✅
})

// 2. 图片路径问题
// 本地图片
<image src="/images/logo.png" />

// 网络图片需要配置域名
// 开发环境：开发者工具 → 详情 → 本地设置 → 不校验合法域名
// 生产环境：微信公众平台 → 开发 → 开发设置 → 服务器域名

// 3. 跨域问题
// 小程序没有跨域概念，但需要配置合法域名

// 4. 包大小超限
// 使用分包加载
{
  "subPackages": [
    {
      "root": "packageA",
      "pages": [...]
    }
  ]
}

// 主包 < 2MB
// 整个小程序所有包大小 < 20MB

// 5. 授权问题
// 使用 button open-type 进行授权
<button open-type="getUserInfo" bindgetuserinfo="getUserInfo">
  授权登录
</button>

// 6. iOS 和 Android 兼容性
// 注意不同平台的 API 差异
const systemInfo = wx.getSystemInfoSync()
const isIOS = systemInfo.platform === 'ios'
```

## Best Practices Summary

1. **项目结构** - 清晰的目录结构，职责分离
2. **代码规范** - 统一的命名和代码风格
3. **性能优化** - 合理使用 setData，避免频繁更新
4. **错误处理** - 完善的错误捕获和用户提示
5. **状态管理** - 集中式状态管理，数据流清晰
6. **组件复用** - 提取公共组件，提高复用性
7. **API 封装** - 统一的请求封装，便于维护
8. **用户体验** - 合理的加载提示和错误反馈
9. **安全性** - token 管理，数据验证
10. **测试** - 充分测试，覆盖各种场景

## When to Use This Skill

Invoke this skill when:
- Creating new WeChat Mini Programs
- Developing mini program features
- Optimizing mini program performance
- Fixing mini program bugs
- Implementing mini program APIs
- Designing mini program architecture
- Migrating H5 to mini program

This skill provides comprehensive WeChat Mini Program development guidance covering all aspects from project setup to deployment and optimization.
