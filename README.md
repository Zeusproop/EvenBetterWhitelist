# EvenBetterWhitelist - Plugin
This repository is the official **Issue Tracker** for **WhitelistPluginButBetter**.  **Note:** The source code for this project is **All Rights Reserved** and is not hosted here. Use this space exclusively to report bugs, suggest features, or request support. Please check existing issues before posting.  Thanks for helping improve the plugin!



[![Modrinth](https://img.shields.io/badge/Modrinth-Project-green?style=flat-square&logo=modrinth)](https://modrinth.com/plugin/even-better-whitelist)
[![Minecraft](https://img.shields.io/badge/Minecraft-1.21.x-brightgreen?style=flat-square)](https://papermc.io/)
![Discord](https://img.shields.io/badge/Discord-Integration-5865F2?style=flat-square&logo=discord)

A modern whitelist system for Minecraft Paper 1.21.x with Discord integration. When a player attempts to join and is not whitelisted, a structured request is posted to your Discord channel. Admins can accept, deny, or ban directly from Discord using buttons.

![Minecraft](https://img.shields.io/badge/Minecraft-1.21.x-green?style=flat-square&logo=minecraft)
![Paper](https://img.shields.io/badge/Paper-API-blue?style=flat-square)
![Discord](https://img.shields.io/badge/Discord-Integration-5865F2?style=flat-square&logo=discord)
![Java](https://img.shields.io/badge/Java-21-orange?style=flat-square&logo=openjdk)


### Installation Steps

1. Place `WhitelistPluginButBetter-1.0.0.jar` in your server's `plugins/` folder
2. Start/restart your server
3. Edit `plugins/WhitelistPluginButBetter/config.yml`
4. Download [GeoLite2-City.mmdb](https://dev.maxmind.com/geoip/geolite2-free-geolocation-data) and place it in the plugin folder
5. Reload with `/wlp reload` or restart the server

## 🤖 Discord Bot Setup

### 1. Create a Discord Application

1. Go to [Discord Developer Portal](https://discord.com/developers/applications)
2. Click "New Application" and give it a name
3. Go to "Bot" section and click "Add Bot"
4. Copy the **Bot Token** (keep this secret!)
5. Enable these **Privileged Gateway Intents**:
   - Message Content Intent

### 2. Invite the Bot

1. Go to "OAuth2" → "URL Generator"
2. Select scopes: `bot`, `applications.commands`
3. Select permissions: `Send Messages`, `Embed Links`, `Read Message History`
4. Copy the generated URL and open it to invite the bot

### 3. Get Your IDs

Enable Developer Mode in Discord (Settings → Advanced → Developer Mode)

- **Server ID**: Right-click your server → Copy Server ID
- **Channel ID**: Right-click the channel → Copy Channel ID
- **Admin User ID**: Right-click yourself → Copy User ID

### 4. (Optional) Create a Webhook

1. Right-click your channel → Edit Channel
2. Go to Integrations → Webhooks
3. Create a webhook and copy the URL

## ⚙️ Configuration

```yaml
discord:
  bot-token: "YOUR_BOT_TOKEN_HERE"
  webhook-url: "YOUR_WEBHOOK_URL_HERE"
  server-id: "YOUR_SERVER_ID_HERE"
  channel-id: "YOUR_CHANNEL_ID_HERE"
  admin-user-ids: "USER_ID_1,USER_ID_2"
  ping-admins: true

cooldowns:
  deny-cooldown: 60        # Minutes before denied player can try again
  pending-cooldown: 5      # Minutes between join attempts while pending
  ip-cooldown: 10          # Minutes before same IP can make new request

geoip:
  enabled: true
  database-path: "plugins/WhitelistPluginButBetter/GeoLite2-City.mmdb"
```

See the full [config.yml](src/main/resources/config.yml) for all options!

## 📝 Commands

| Command | Description |
|---------|-------------|
| `/wlp accept <player>` | Accept a pending whitelist request |
| `/wlp deny <player>` | Deny a pending whitelist request |
| `/wlp pending` | View all pending requests |
| `/wlp list [page]` | View whitelisted players |
| `/wlp add <player>` | Manually add a player to whitelist |
| `/wlp remove <player>` | Remove a player from whitelist |
| `/wlp info <player>` | View detailed player info |
| `/wlp status` | View plugin status |
| `/wlp reload` | Reload configuration |

## 🔐 Permissions

| Permission | Description | Default |
|------------|-------------|---------|
| `whitelistplugin.admin` | Access to all commands | OP |
| `whitelistplugin.bypass` | Bypass whitelist check | OP |

## Discord Embed Preview

When a player tries to join, admins receive an embed like this:

```
┌─────────────────────────────────────────┐
│ New Whitelist Request                   │
├─────────────────────────────────────────┤
│ Player: Steve                           │
│ UUID: 069a79f4-...                      │
│ IP: ||192.168.1.1||                     │
│ Location: New York, United States       │
│ Client: fabric                          │
│ Language: en_US                         │
│ Requested: 2024-01-15 14:30 UTC         │
│                                         │
│ Same IP as: Alex (1 other request)      │
│                                         │
│ [Accept]  [Deny]  [Ban]                 │
└─────────────────────────────────────────┘
```

## 🗂️ File Structure

```
plugins/WhitelistPluginButBetter/
├── config.yml              # Main configuration
├── whitelist.json          # Whitelisted players
├── pending-requests.json   # Pending requests
├── denied-players.json     # Denied players (for cooldowns)
├── requests.log            # Action log
└── GeoLite2-City.mmdb      # GeoIP database (you provide this)
```

## Troubleshooting

### Bot not responding to buttons?
- Make sure the bot token is correct
- Ensure the bot has permissions in the channel
- Check console for connection errors

### GeoIP not working?
- Download GeoLite2-City.mmdb from MaxMind
- Place it in the plugin folder
- Check the path in config.yml

### Players not being kicked?
- Make sure the plugin is loaded (`/plugins`)
- Check if player has `whitelistplugin.bypass` permission
- Look for errors in console

### Seeing Private IP (10.x.x.x, 192.168.x.x)?
This happens when using a **reverse proxy** (BungeeCord, Velocity, TCPShield, etc.)

**For BungeeCord:**
1. Set `bungeecord: true` in `spigot.yml`
2. Restart the server

**For Velocity:**
1. Edit `config/paper-global.yml`
2. Set `proxies.velocity.enabled: true`
3. Set the `secret` to match Velocity's forwarding secret
4. Restart the server

**For TCPShield/Other:**
Follow your proxy's documentation for IP forwarding.

### Client/Language shows "N/A (Pre-login)"?
This is expected! The plugin intercepts players **before** they fully connect, so client brand and locale data isn't available yet. This is a Minecraft limitation, not a bug.

The data we DO capture (IP, UUID, GeoIP location) is still very useful for identifying players and alts!

## 📜 License

All Rights Reserved.Any modifications without informing is illegal.

Made for the Minecraft community!
