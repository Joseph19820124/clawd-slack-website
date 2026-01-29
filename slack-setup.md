# Clawdbot Slack 接入指南

好消息！clawdbot 原生支持 Slack，不需要重新部署。

---

## 当前状态

```
c6g-large-clawdbot (3.140.154.43)
├── clawdbot 2026.1.24-3 运行中
├── Gateway 端口: 18789
└── Channels:
    ├── Telegram ✅ 已配置 (@Josephclawdbot)
    └── Slack ✅ 已配置
```

---

## 添加 Slack 只需一条命令

```bash
clawdbot channels add \
  --channel slack \
  --bot-token xoxb-xxxxx \
  --app-token xapp-xxxxx
```

---

## 接入步骤

### 1. 创建 Slack App（在 Slack 网站操作）

1. 访问 https://api.slack.com/apps
2. 点击 Create New App → From scratch
3. 输入 App 名称（如 Moltbot），选择 Workspace

### 2. 配置 App 权限

OAuth & Permissions → Bot Token Scopes 添加：

- `app_mentions:read`
- `chat:write`
- `channels:history`
- `im:history`
- `im:write`
- `users:read`

### 3. 启用 Socket Mode

Settings → Socket Mode → Enable

- 创建 App-Level Token，选择 `connections:write` scope
- 获得 `xapp-...` token

### 4. 启用 Event Subscriptions

Features → Event Subscriptions → Enable

Subscribe to bot events:

- `app_mention`
- `message.im`

### 5. 安装 App 到 Workspace

OAuth & Permissions → Install to Workspace

- 获得 `xoxb-...` Bot Token

### 6. 在 EC2 上添加 Slack channel

```bash
ssh -i ~/.ssh/id_ed25519_target ec2-user@3.140.154.43

clawdbot channels add \
  --channel slack \
  --bot-token "xoxb-你的bot-token" \
  --app-token "xapp-你的app-token"

# 重启 gateway 使配置生效
clawdbot gateway restart
```

### 7. 验证

```bash
clawdbot channels list
clawdbot channels status
```

---

要我帮你执行 Step 6 吗？你需要先完成 Slack App 创建（Step 1-5）并获取两个 token。
