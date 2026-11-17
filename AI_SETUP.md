# HANDOVER · AI 接入

本项目网页仍由 GitHub Pages 托管。AI 请求通过 Cloudflare Worker 中转，Qwen API Key 只保存在 Worker Secret 中，不放进网页代码。

## 部署一次（需要你本人登录账号）

### 1. 开通 Qwen API
1. 登录 Alibaba Cloud Model Studio，选择 Singapore 区域。
2. 在 API Key 页面创建一个 API Key。
3. 打开 Model Usage / Free Quota，找到支持免费额度的模型，启用 Free Quota Only。官方说明：免费额度仅适用于符合条件的新用户、Singapore 区域及指定模型；额度通常有效 90 天。开启后额度耗尽时请求会停止，避免自动转付费。
4. 不要把 API Key 发在聊天里，也不要粘贴到 GitHub。

### 2. 部署 Cloudflare Worker
1. 登录 Cloudflare Dashboard，进入 Workers & Pages。
2. 创建 Worker，名称可用 handover-iris-api。
3. 在 Worker 的代码编辑器中，把本仓库 worker.js 的完整内容粘贴进去并 Deploy。
4. 打开 Worker 的 Settings → Variables and Secrets → Add。
5. 类型选择 Secret，名称填写 QWEN_API_KEY，值粘贴刚创建的 Qwen API Key，然后保存并重新 Deploy。
6. Worker 的免费计划目前有每日请求额度；具体限制以 Cloudflare 官方文档为准。

### 3. 把 Worker 地址接回游戏
部署完成后，Cloudflare 会给出类似 https://handover-iris-api.<你的子域>.workers.dev 的地址。将该地址发给我即可；我会把它接入 index.html 并更新 GitHub Pages。

## 安全与限制
- API Key 只存在 Cloudflare Secret 中；不要放入 index.html。
- Worker 只接受游戏 GitHub Pages 域名的浏览器 Origin，并限制请求体和上下文长度。这是轻量演示级保护，不是强身份认证；公开网页仍可能被有技术能力的人绕过 Origin 限制直接调用。分享给朋友前建议观察用量。
- 如果免费额度耗尽或过期，AI 对话会暂时不可用；不会自动改用付费额度（前提是已启用 Free Quota Only）。
- 不需要修改现有游戏 UI，也不需要朋友注册账号。
