# AI Builders Digest

自動抓取 AI 領域頂級 Builders 的 Twitter / 播客 / 部落格更新，
用 Claude remix 成雙語精華摘要，通過 Telegram / Email 定時發送。

基於 [follow-builders](https://github.com/zarazhangrui/follow-builders) 核心 Feed，
透過 GitHub Actions 實現全自動、零維護的每日 Digest。

---

## 功能架構

```
[GitHub Actions]
    │
    ├─ prepare-digest.js    → 抓取 feed-x.json / feed-podcasts.json / feed-blogs.json
    │                           + 組裝 config + prompts → JSON blob
    │
    ├─ Claude API            → 讀取 JSON，生成雙語摘要文稿
    │
    └─ deliver.js            → 發送至 Telegram / Email
```

## 設定步驟

### 1. Fork 或 Clone 此 Repo

```bash
git clone https://github.com/YOUR_USERNAME/ai-builders-digest.git
cd ai-builders-digest
```

### 2. 新增 GitHub Secrets

在 Repo → **Settings → Secrets and variables → Actions** 中新增：

| Secret 名稱 | 說明 |
|---|---|
| `ANTHROPIC_API_KEY` | Anthropic API Key（用於生成摘要）|
| `TELEGRAM_BOT_TOKEN` | Telegram Bot Token（@BotFather 取得）|
| `TELEGRAM_CHAT_ID` | 你的 Telegram Chat ID（發送給 @userinfobot 取得）|
| `RESEND_API_KEY` | Resend API Key（用於 Email 發送）|
| `EMAIL_TO` | 接收郵箱地址 |

### 3. （可選）自訂配置

複製 `config.example.json` 為 `config.json`，或直接透過 workflow 中的環境變量覆蓋：

```yaml
env:
  CONFIG_LANGUAGE: bilingual   # bilingual | en | zh-CN
  CONFIG_FREQUENCY: daily
  CONFIG_DELIVERY_METHOD: both # telegram | email | both | stdout
```

### 4. 手動觸發測試

前往 Repo → **Actions → AI Builders Digest → Run workflow**

---

## 工作流程

| 觸發方式 | 時間 |
|---|---|
| Cron（自動）| 每天 06:00 UTC |
| workflow_dispatch（手動）| 任意時間 |

---

## 自訂 Prompt

如需修改摘要風格，可透過環境變量注入自訂 Prompt：

```yaml
env:
  PROMPT_DIGEST_INTRO: |
    你是一位頂級 AI 科技博主，風格犀利有趣...
```

---

## License

MIT
