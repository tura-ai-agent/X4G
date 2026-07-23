[فارسی](README.md) | **English**

YouTube channel: [https://www.youtube.com/@X4GHUB](https://www.youtube.com/@X4GHUB)

# 🚀 X4G

A fast, modern gateway for tunneling VLESS over WebSocket and XHTTP, plus an HTTP Proxy, with a polished management dashboard, **Telegram management bot**, professional subscription pages, and support for creating dedicated links with traffic, speed, and IP limits.

## ✨ Features

* 🔌 VLESS tunneling over multiple selectable transports: **WebSocket**, **XHTTP (packet-up)**, and **XHTTP (stream-up)**
* 🌐 Built-in HTTP Proxy
* 📊 Complete management dashboard (statistics, hourly traffic charts, live connections, and activity and error logs)
* 🔗 Unlimited link management with dedicated traffic limits (KB/MB/GB)
* 🚦 Per-configuration speed limits (Bandwidth Throttling), measured in Mbps
* ✅ Instantly enable or disable each link, with automatic expiration based on days
* 📱 QR Code output for every link and subscription
* 🛡️ Manually configurable Fingerprint (uTLS) and ALPN for each configuration
* 🔢 Manually configurable connection port for each configuration (not limited to 443)
* 👥 Limit the number of concurrent IPs or users for each configuration
* 🗂 **Subscription groups**: put multiple configurations into one group and get a polished subscription link for all of them, with a public page that can be password-protected
* 💾 Persist state on disk, rather than only in memory, so it survives service restarts, provided that a persistent Volume is attached to the data path
* 🤖 Optional **Telegram management bot** for fully managing configurations and subscription groups without opening the web panel

## 1️⃣ Fork on GitHub

First, click the Fork button to copy this repository to your account.

## 2️⃣ Deploy on Railway

1. Go to [Railway.app](https://railway.app/).
2. Click New Project → Deploy from GitHub repo.
3. Select the forked repository.
4. Railway deploys the project automatically.

💡 After deployment, enable a Public Domain for your service in the Railway settings so that the `RAILWAY_PUBLIC_DOMAIN` variable is set correctly.

### 💾 Persistent storage (Volume)

The service state (configurations, usage statistics, subscription groups, and admin password) is stored in `x4g_state.json` under `DATA_DIR` (default: `/data`). To prevent this information from being lost after a restart or a new Railway deployment, create a **Volume** and attach it to the same path (`/data`) from the service settings under Volumes. Without this, the information remains only in the current container and is deleted when the container is replaced.

## 3️⃣ Connect to configurations

After a successful deployment:

1. Go to `https://your-app.up.railway.app/dashboard`.
2. On the overview dashboard, view and copy the default VLESS link, which has no limits.
3. Import the link into your preferred client (v2rayNG, NekoBox, Streisand, and so on).
4. To create separate links with traffic, speed, IP, and other limits, go to the link management section.

## 🔧 Manual settings for each configuration

When creating or editing a configuration, you can configure each of the following separately:

* **Protocol/transport**: `vless-ws` (VLESS over WebSocket) or one of the XHTTP modes (`xhttp-packet-up` / `xhttp-stream-up`)
* **Fingerprint (uTLS)**: chrome / firefox / safari / ios / android / edge / 360 / qq / random / randomized
* **ALPN**: the protocol default, or a manual value such as `h2,http/1.1` or `http/1.1`
* **Connection port**: any port from 1 to 65535 (not limited to 443) — note that this port must actually be open and accessible on your domain or service
* **Traffic limit**: measured in KB/MB/GB (0 = unlimited)
* **Speed limit**: measured in Mbps and set independently for each configuration (0 = unlimited)
* **Concurrent IP limit**: the number of IPs or users allowed to use the same configuration simultaneously (0 = unlimited)
* **Expiration**: the number of days for which the configuration remains valid (no value = no expiration)

## 🗂 Subscription groups (professional subscription links)

Instead of a simple subscription link (`/sub/{uuid}`, which returns only the configuration's base64 text), you can place multiple configurations in a **subscription group** to create a polished **public page** (`/p/{uuid_key}`). The page visually shows the configuration list, each configuration's status and usage, and QR Codes, and it can be password-protected when needed. To create this link:

1. Go to the “Subscription groups” section in the panel, or use the Telegram bot.
2. Create a new group.
3. Add the desired configuration or configurations to that group.
4. Copy the group's public-page link from there.

## 🤖 Telegram management bot (optional)

To manage configurations through Telegram without opening the web panel:

1. Add the following two environment variables in the Railway service settings:
   * `TELEGRAM_BOT_TOKEN` — the bot token from [@BotFather](https://t.me/BotFather)
   * `TELEGRAM_ADMIN_IDS` — the numeric Telegram IDs of authorized admins, separated by commas (for example, `123456789,987654321`)
2. After restarting the service, send `/start` to the bot.

Bot capabilities:

* 📋 List, view, enable or disable, and delete configurations
* ➕ Create a configuration with a step-by-step wizard (label → protocol → Fingerprint → ALPN → port → traffic limit → speed limit → IP limit → expiration days)
* 🔗 Get the VLESS connection link and subscription link for each configuration
* 🗂 Create and manage subscription groups and get professional subscription links directly in the chat

## ⚙️ Environment variables

| Variable | Description | Default |
|---|---|---|
| `ADMIN_PASSWORD` | Password used to sign in to the web dashboard | `X4GKING` |
| `SECRET_KEY` | Session and password signing key; if unset, it is generated automatically and stored on disk | — |
| `DATA_DIR` | State storage path (requires a persistent Volume) | `/data` |
| `TELEGRAM_BOT_TOKEN` | Telegram bot token (optional) | — |
| `TELEGRAM_ADMIN_IDS` | Numeric Telegram IDs of authorized admins, separated by commas (optional) | — |
| `RAILWAY_PUBLIC_DOMAIN` | Public service domain; configured automatically by Railway | `localhost` |

## ⚠️ Important note

Although the service state (configurations, usage statistics, and subscription groups) is stored on disk, **without a persistent Volume attached to `DATA_DIR`**, the information remains only in the current container and is lost when Railway replaces or rebuilds the container. Be sure to attach a Volume as described in the “Persistent storage” section above.

---

X4G · Support: [group link](https://t.me/x4g_group)
