# cordova-plugin-aliyun-push

基于 https://github.com/log2c/cordova-plugin-log2c-aliyun-push 修改而来

Cordova 阿里云移动推送插件，现包含`小米`、`华为`、`OPPO`、`VIVO`、`魅族` 厂商辅助通道

## 依赖说明

- Android:

  截止日期`2022/04/15`为止最新的依赖

- iOS:
  截止日期`2020/03/05`为止最新的依赖

## 安装

- 安装插件

  ```
    ionic cordova plugin add ***(本地地址) \`
  ```

  ```
    ionic cordova plugin add https://github.com/TianKong-0512/cordova-plugin-aliyun-push \
    --variable ANDROID_APP_KEY="***" \
    --variable ANDROID_APP_SECRET="***" \
    --variable IOS_APP_KEY="***" \
    --variable IOS_APP_SECRET="***" \
    --variable HUAWEI_APPID="***" \
    --variable MIPUSH_APPID="***" \
    --variable MIPUSH_APPKEY="***" \
    --variable VIVOPUSH_APPID="***" \
    --variable VIVOPUSH_APPKEY="***" \
    --variable OPPOPUSH_APPKEY="***" \
    --variable OPPOPUSH_APPSECRET="***" \
    --variable MZPUSH_APPID="***" \
    --variable MZPUSH_APPKEY="***" \
    --variable CHANNEL_ID="1"
  ```

  - 注意
    - 将`*`号替换成你自己申请的密钥信息,如无则不填写或保持`*`号(不影响正常运行)
    - `CHANNEL_ID`对应`Android 8.0`的通知通道,根据实际情况填写(`Android 8.0`之后必须的通道 id)

- `Android`端配置(必要)

    <div style="color:red">**插件默认没有阿里云推送初始化,所以要依据下列方式进行初始化**</div>

  在你项目的`config.xml`中添加

  ```xml
      <platform name="android">
          <!-- ↓↓↓↓↓↓↓ 以下内容 ↓↓↓↓↓↓↓ -->
          <edit-config file="app/src/mainAndroidManifest.xml" mode="merge" target="manifest/application"
                  xmlns:android="http://schemas.androidcom/apk/res/android">
                  <application android:name="com.alipush.PushApplication" />
          </edit-config>
          <!-- ↑↑↑↑↑↑↑ 以上内容 ↑↑↑↑↑↑↑ -->
      </platform>
  ```

## 注意

- `Android`

  - 杀死 App 点击通知无法打开 APP 的,后端推送时添加 `AndroidExtParameters {open_type:"application"}`
  - 修改 PushApplication.java 中自己应用的包名。
  - 因为 android 应用商店的隐私协议需要在同意后才能获取 某些信息，所以添加了 initPush 初始化。
  - 打包报错的需要修改 build.gradle 中 com.android.tools.build:gradle:3.3.3 版本号 3.3.0 是不行，我暂时使用的 3.3.3

- `IOS`
  - ios 无需调用 initPush 初始化。

## 使用

### `ionic`中使用(可选)

为`typescript`大法好添加了`ts`声明,需要的可以根据下列步骤操作

1. 根据上面步骤添加`plugin`
1. 请切换到**[@ionic-native](https://github.com/TianKong-0512/cordova-plugin-aliyun-push/tree/%40ionic-native)**分支,根据`README.md`操作使用

### API

```
    /**
     * 获取设备唯一标识deviceId，deviceId为阿里云移动推送过程中对设备的唯一标识（并不是设备UUID/UDID）
     * @param  {Function} successCallback 成功回调
     * @param  {Function} errorCallback   失败回调
     * @return {void}
     */
    getRegisterId: function(successCallback, errorCallback)

    /**
     * 阿里云推送绑定账号名
     * @param  {string} account         账号
     * @param  {Function} successCallback 成功回调
     * @param  {Function} errorCallback   失败回调
     * @return {void}
     */
    bindAccount: function(account, successCallback, errorCallback)

    /**
     * 阿里云推送解除账号名,退出或切换账号时调用
     * @param  {Function} successCallback 成功回调
     * @param  {Function} errorCallback   失败回调
     * @return {void}
     */
    unbindAccount: function(successCallback, errorCallback)

    /**
     * 阿里云推送绑定标签
     * @param  {string[]} tags            标签列表
     * @param  {Function} successCallback 成功回调
     * @param  {Function} errorCallback   失败回调
     * @return {void}
     */
    bindTags: function(tags, successCallback, errorCallback)

    /**
     * 阿里云推送解除绑定标签
     * @param  {string[]} tags            标签列表
     * @param  {Function} successCallback 成功回调
     * @param  {Function} errorCallback   失败回调
     * @return {void}
     */
    unbindTags: function(tags, successCallback, errorCallback)

    /**
     * 阿里云推送解除绑定标签
     * @param  {Function} successCallback 成功回调
     * @param  {Function} errorCallback   失败回调
     * @return {void}
     */
    listTags: function(successCallback, errorCallback)

    /**
     * 添加别名
     * @param  {Function} successCallback 成功回调
     * @param  {Function} errorCallback   失败回调
     * @return {void}
     */
    addAlias: function (alias, successCallback, errorCallback)

    /**
     * 解绑别名
     * @param  {Function} successCallback 成功回调
     * @param  {Function} errorCallback   失败回调
     * @return {void}
     */
    removeAlias: function (alias, successCallback, errorCallback)

    /**
     * 获取别名列表
     * @param  {Function} successCallback 成功回调
     * @param  {Function} errorCallback   失败回调
     * @return {void}
     */
    listAliases: function (successCallback, errorCallback)

    /**
      * 没有权限时，请求开通通知权限，其他路过
      * @param  string msg  请求权限的描述信息
      * @param {} successCallback
      * @param {*} errorCallback
      */
    requireNotifyPermission:function(msg,successCallback, errorCallback)

    /**
    * 阿里云推送消息透传回调
    * @param  {Function} successCallback 成功回调
    */
    onMessage:function(sucessCallback) ;

    # sucessCallback:调用成功回调方法，注意没有失败的回调，返回值结构如下：
    #json: {
      type:string 消息类型,
      title:string '阿里云推送',
      content:string '推送的内容',
      extra:string | Object<k,v> 外健,
      url:路由（后台发送推送时，在ExtParameters参数里写入url如{url:'demoapp://...'}）
    }

    #消息类型
    {
      message:透传消息，
      notification:通知接收，
      notificationOpened:通知点击，
      notificationReceived：通知到达，
      notificationRemoved：通知移除，
      notificationClickedWithNoAction：通知到达，
      notificationReceivedInApp：通知到达打开 app
    }

```

## 常见问题

1. `Android 8.0`以上无法获取到`Token`
   检查是否配置了`network_security_config.xml`信息，具体百度了解

1. `iOS`无法获取到`Token`
   `Xcode`中确认开启以下两项
   ![](https://github.com/TianKong-0512/cordova-plugin-aliyun-push/blob/master/screenshoot/iOS_notification_config.png)
