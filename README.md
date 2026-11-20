# 小通

自带 DeepSeek API Key 的开源聊天客户端。**一个 HTML 文件，没有后端，没有中转服务器。**

小通不卖会员、不收钱、不代收任何费用。你用自己的 Key 直连 DeepSeek 官方接口，费用由你自己付给 DeepSeek。

## 特点

- **纯静态**：`index.html` + 几个图片，没有任何构建步骤和依赖，扔到任何静态托管上就能跑
- **流式输出**：边生成边显示，不是等一串字再蹦出来
- **深度思考**：一键切到 `deepseek-reasoner`，思考过程可以展开看
- **对话留在本地**：Key 和聊天记录只存在浏览器的 localStorage 里，刷新还在，关掉也不上传
- **手机能用**：响应式布局，iOS/Android 浏览器打开即可；支持「添加到主屏幕」
- **深色模式**：跟随系统

## 直接用

打开部署好的网址，点右上角 ⚙ 填入自己的 DeepSeek Key 就能聊。

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
3. 复制 `sk-` 开头的那串，粘到小通的设置里

DeepSeek 按用量扣费，普通聊天一天通常几毛钱。余额不足时小通会直接提示（402）。

## 安全说明

- Key **只**保存在你自己浏览器的 localStorage 里，页面不会把它发给除 `api.deepseek.com` 以外的任何地方
- localStorage 是明文的：不要在公共电脑上保存 Key
- 这个项目本身不收集任何数据，没有统计脚本，没有第三方 CDN 依赖

## 和 DeepSeek 官方的关系

小通是第三方客户端，与 DeepSeek 官方无关，也不代表官方立场。
「DeepSeek」是其权利人的商标，本项目仅用于说明所调用的接口。
喜欢这个工具的话，也请支持官方的网页版和 App。

## 技术说明

- 无框架、无打包、无 npm 依赖：一个 `index.html` 里包含全部 HTML/CSS/JS
- 流式解析 SSE：`fetch` + `ReadableStream` + `TextDecoder`
- 断点式极简 Markdown 渲染（代码块 / 标题 / 列表 / 粗体 / 行内代码 / 链接），内容一律先转义再渲染
- 请求直接从浏览器发往 `https://api.deepseek.com/chat/completions`（官方接口允许跨域）

## 许可

[MIT](./LICENSE)
