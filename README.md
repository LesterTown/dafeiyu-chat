# 大肥鱼对话版

自带 DeepSeek API Key 的开源聊天客户端。**一个 HTML 文件，没有后端，没有中转服务器。**

大肥鱼不卖会员、不收钱、不代收任何费用。你用自己的 Key 直连 DeepSeek 官方接口，费用由你自己付给 DeepSeek。

## 特点

- **纯静态**：`index.html` + 几个图片，没有任何构建步骤和依赖，扔到任何静态托管上就能跑
- **流式输出**：边生成边显示，不是等一串字再蹦出来
- **深度思考**：输入框左边的开关，打开就切到 `deepseek-reasoner`，思考过程可以展开看
- **联网搜索**：同一个开关栏，先搜网页再作答，回答里标 [1][2] 并附「参考 N 个网页」，走的是 DeepSeek 官方 Search 接口，用你自己的 Key 计费（每 1000 次成功搜索 5 元）
- **左侧历史会话**：所有对话分组留在左边，点一下就接着聊；右上角 ＋ 开新对话；左上角 « 可以把整个侧栏收起来，收起后顶栏会出现 ☰ 再展开（会记住你的选择）
- **对话留在本地**：Key 和聊天记录只存在浏览器的 localStorage 里，刷新还在，关掉也不上传
- **手机能用**：响应式布局，iOS/Android 浏览器打开即可；支持「添加到主屏幕」
- **账户面板**：设置里直接显示还剩多少钱（调官方 `/user/balance`），点了「去充值」「去拿 Key」直接开对应网页；余额不足（402）的报错里也带充值链接
- **深浅色**：默认跟随系统，右上角的 ☾ / ☀ 可以手动切，选择会记住

## 直接用

在线版：**<https://lestertown.github.io/dafeiyu-chat/>**

打开后用右上角 ⚙ 填自己的 DeepSeek Key 就能聊，不需要注册任何账号。

> 提示：`github.io` 在国内访问不稳定，打不开时可以本地打开 `index.html`，或自己部署一份。

## 装成 App

**安卓**：去 [Releases](https://github.com/LesterTown/dafeiyu-chat/releases) 下载最新的 `dafeiyu-chat-x.y.z.apk`，用手机打开安装。

第一次装会提示「不允许安装未知来源应用」，按提示给浏览器开个权限再装一次就行。小米/华为/OPPO 可能提示「未经安全检测」，点「继续安装」。

APK 里打包了完整的界面，断网也能打开，联网才能聊天。

**iPhone / iPad**：苹果不允许从网页装 App，用 Safari 打开在线版，点底部「分享」→「添加到主屏幕」，桌面会出现一条鱼，点开是全屏的，跟 App 差不多。

系统限制：这种装法拿不到推送通知，后台也会被系统回收，但聊天本身不受影响。

## 自己打包安卓

需要 JDK 17～21 和 Android SDK：

```bash
npm install
npm run apk                     # 生成 android/app/build/outputs/apk/debug/app-debug.apk
```

正式包用 `./gradlew assembleRelease`，签名配置放 `android/keystore.properties`（已在 `.gitignore` 里，不会提交）：

```properties
storeFile=/绝对路径/你的.jks
storePassword=...
keyAlias=...
keyPassword=...
```

## 自己部署（GitHub Pages，免费 5 分钟）

1. Fork 这个仓库
2. 仓库 **Settings → Pages**
3. Source 选 `Deploy from a branch`，Branch 选 `main` / 目录 `/ (root)`，保存
4. 等一分钟，访问 `https://<你的用户名>.github.io/<仓库名>/`

不需要域名、不需要服务器、不需要备案。

想用自己的域名也可以：在 Pages 里填 Custom domain，然后在域名商处加一条 CNAME 记录。

## 怎么拿 Key

1. 打开 <https://platform.deepseek.com> 并登录
2. 左侧「API Keys」→「创建 API Key」
3. 复制 `sk-` 开头的那串，粘到大肥鱼的设置里

DeepSeek 按用量扣费，普通聊天一天通常几毛钱。余额不足时大肥鱼会直接提示（402）。

## 安全说明

- Key **只**保存在你自己浏览器的 localStorage 里，页面不会把它发给除 `api.deepseek.com` 以外的任何地方
- localStorage 是明文的：不要在公共电脑上保存 Key
- 这个项目本身不收集任何数据，没有统计脚本，没有第三方 CDN 依赖
- 只有 8 个静态文件，没有后端、没有数据库、没有账号体系，作者手上没有任何访客数据
- 仓库里也不含任何个人信息：提交使用 GitHub 的匿名邮箱，代码里没有本地路径


## 和 DeepSeek 官方的关系

大肥鱼对话版是第三方客户端，与 DeepSeek 官方无关，也不代表官方立场。
「DeepSeek」是其权利人的商标，本项目仅用于说明所调用的接口。
喜欢这个工具的话，也请支持官方的网页版和 App。

## 技术说明

- 无框架、无打包、无 npm 依赖：一个 `index.html` 里包含全部 HTML/CSS/JS
- 流式解析 SSE：`fetch` + `ReadableStream` + `TextDecoder`
- 断点式极简 Markdown 渲染（代码块 / 标题 / 列表 / 粗体 / 行内代码 / 链接），内容一律先转义再渲染
- 请求直接从浏览器发往 `https://api.deepseek.com/chat/completions`（官方接口允许跨域）
- 联网搜索走 `POST https://api.deepseek.com/v1/tools/search`，同样允许跨域；搜索结果只在本轮塞进上下文，不写进聊天记录

## 许可

[MIT](./LICENSE)
