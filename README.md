# 🤖 CF-Workers AutoCheckin (安全升级版)
![签到面板预览](./p.png)

本项目是一个基于 Cloudflare Workers 的机场自动签到工具。
**新版本核心升级**：自带美观的 Web 可视化管理面板、彻底隔离面板登录密码与真实业务密码、告别明文泄露，并仅支持高安全的 Telegram 官方 API 推送。

## ✨ 核心特性
- 🛡️ **极致安全**：引入独立的 `PANEL_PASS`（面板密码），你的真实机场密码仅用于后台静默签到，杜绝前端及 URL 泄露风险。
- 📱 **Web 管理面板**：无需再通过记住杂乱的 URL 路径来触发，直接访问 Worker 域名即可进入可视化控制台。
- 🔔 **纯净 TG 推送**：剔除所有第三方中转 API，完全通过 Telegram 官方接口推送，并在推送内容中对密码自动打码（`a***z`）。

---

## 🚀 部署与使用

### 1️⃣ 访问管理面板
部署完成后，直接在浏览器访问你的 Worker 域名：
* 示例地址: `https://qd.your-domain.workers.dev/`
* **默认登录密码**: `123456`（强烈建议在环境变量中配置 `PANEL_PASS` 进行修改）

### 2️⃣ 手动执行签到 & 测试 TG
登录面板后，你可以直接在左侧边栏查看当前配置状态，并点击：
* **[手动执行签到]**：实时查看签到日志与结果回报。
* **[测试 TG 推送]**：验证你的 Telegram 机器人是否配置正确。

### 3️⃣ ⏰ 设置定时自动签到 (Cron 触发器)
想要每天全自动运行，请在 Cloudflare 仪表盘中配置：
1. 前往 **设置** > **触发事件** > **+添加** > **Cron 触发器**
2. 切换到 **Cron 表达式** 模式。
3. 输入你想要执行的时间表达式并保存。

> [!CAUTION]
> **时差注意**：Cloudflare 使用的是 UTC 标准时间，与北京时间（UTC+8）有 **8 小时**的时差！
> 举例：如果你想在 **北京时间中午 12:30** 执行签到，对应的 UTC 时间是凌晨 04:30，你的 Cron 表达式应填为：`30 4 * * *`。

---

## 📋 环境变量 (Variables) 说明

在 Cloudflare Worker 的 **设置** -> **变量和机密** 中配置以下参数：

| 变量名 | 示例 | 必填 | 备注 |
| :--- | :--- | :---: | :--- |
| `DOMAIN` / `JC` | `https://jichang.com` | ✅ | **机场域名** (带不带 https:// 均可，脚本会自动补全) |
| `USER` / `ZH` | `admin@google.com` | ✅ | **机场签到邮箱** |
| `PASS` / `MM` | `MyRealPassword123` | ✅ | **机场真实密码** (用于后台静默签到，推送时会自动脱敏) |
| `PANEL_PASS` | `MyPanelPwd` | ❌ | **网页面板登录密码** (如果不填，默认密码为 `123456`) |
| `TGTOKEN` | `689412...:XXXXX` | ❌ | 你的 **Telegram Bot Token** (与 TGID 需同时填写才能启用推送) |
| `TGID` | `6946912345` | ❌ | 接收通知的 **Telegram 用户 ID** |

> [!NOTE]
> **关于 Telegram 推送**：为了保障你的账号隐私，本项目**不再提供内置/第三方的公共推送机器人**。如需 TG 推送，请务必通过 [@BotFather](https://t.me/BotFather) 申请属于你自己的 Bot Token，并通过 [@userinfobot](https://t.me/userinfobot) 获取你的专属 TGID。

---

# 🙏 致谢
原项目逻辑参考自 [CF-Workers-checkin](https://github.com/cmliu/CF-Workers-checkin)，本分支在此基础上进行了深度安全重构与 UI 现代化升级。
