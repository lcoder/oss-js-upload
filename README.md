# oss-js-upload

## 注意
阿里云 STS 下线 GetFederation 接口，导致 oss-js-upload 暂时无法使用，我会尽快更新新的实现方案

## 技术支持
请加旺旺群：1489391962

## 简介
支持在在浏览器端直接上传文件到阿里云 OSS.

注意, 从 0.2.0 版本开始, 将基于 [aliyun sdk js](https://github.com/aliyun-UED/aliyun-sdk-js) 开发. 请注意 demo.html 中的变化.

后续将支持:
- ie8, ie9 浏览器 (8月底)

目前已经支持:
- ie10以上 ie 浏览器, 以及其他主流浏览器(还未全面测试)
- 支持使用阿里云 STS 临时 Token
- 支持文件上传 md5 校验, 保证调用的安全性.
- 文件分块上传, 上传文件大小理论上无限制

## 使用

### 配置 OSS

需要先配置 oss bucket 的 cors. 在开发/调试时, 可以做如下配置

- 将 "来源", "Allowed Header" 配置为 *
- 将 "Method" 全部勾选
- 如果需要进行文件分块上传, 需要将 "Expose Header" 配置为 etag 和 x-oss-request-id

调试通过后, 根据需要最小化相关配置

### 安装

你可以通过如下两种方式中任意一种引入本项目：

#### 1.bower
```sh
$ bower install oss-js-upload --save
```

#### 2.直接下载
1.  下载本项目最新的 [Release](https://github.com/aliyun-UED/oss-js-upload/blob/master/src/oss-js-upload.js)
2.  下载依赖 [aliyun-sdk](https://github.com/aliyun-UED/aliyun-sdk-js/blob/master/dist/aliyun-sdk.min.js)
3.  通过 `<script src=""></script>` 标签引入文件，注意将依赖放在前面

```html
<script src="/path/to/aliyun-sdk/aliyun-sdk.min.js"></script>
<script src="/path/to/oss-js-upload.js"></script>
```

## 使用 demo (需要 Node.js 环境)

- 打开 demoServer.js 填入 accessKeyId 和 secretAccessKey
- 打开 demo.html 填入 bucket 和 endpoint 参数, 其他参数根据需要进行配置
- 执行 npm install，安装 demo 依赖
- 执行 node demoServer.js
- 然后在浏览器中打开 http://localhost:3000/demo.html

在 demo.html 和 demoServer.js 中对各个参数都有详细说明

## 关于 stsToken

将文件上传到 oss 需要进行鉴权, 使用阿里云的 acessKey 和 keySecret 虽然可以成功上传但是会将你 accessKey 和 keySecret 暴露在浏览器端, 因此
不推荐使用, 强烈建议使用 STS token 进行鉴权. aliyunCredential 和 stsToken 必须提供一个, 如果同时提供, 将会使用 stsToken.

阿里云sts 文档 http://docs.aliyun.com/#/pub/ram/sts-user-guide/intro

### 获取 stsToken

stsToken 需要在自己的服务器上生成, 并传递给 oss-js-upload
如果你使用 nodejs 做服务器, 可以使用 aliyun nodejs sdk 获取 stsToken , 示例见 demoServer.js
如果你使用其他语言, 需要使用对应的 sdk 来获取 stsToken
