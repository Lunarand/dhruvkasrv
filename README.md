# Ultimate Minecraft Server (GitHub Actions)

A free 24/7 Minecraft server running on GitHub Actions with Playit.gg tunneling, auto-plugin downloads from Modrinth, and Hugging Face backup storage.

## Required Secrets

Add these in your repo: **Settings → Secrets and variables → Actions**

| Secret | What it does |
|--------|-------------|
| `PLAYIT_SECRET` | Playit.gg agent key — creates a public tunnel so players can join. Get it from [playit.gg](https://playit.gg) dashboard → Agents. |
| `HF_TOKEN` | Hugging Face access token with **Write** role. Create at huggingface.co/settings/tokens. Used to upload/download world backups. |
| `HF_USER` | Your Hugging Face username. |
| `HF_DATASET` | Your HF dataset name (e.g., `minecraft-backup`). Create at huggingface.co → New Dataset → Private. |
| `DISCORD_TOKEN` | Discord bot token for DiscordSRV chat bridge. Optional — skip if you don't want Discord chat. |

## Optional Secrets

| Secret | What it does |
|--------|-------------|
| `SEED` | Custom world seed. Only applied when no backup exists (fresh world). |
| `END_PORTAL` | Set to `false` to disable the End dimension. |
| `ANTI_XRAY` | Set to `false` to disable anti-xray. Enabled by default. |

## How to start

1. Add all secrets above
2. Go to **Actions** tab → **Ultimate Minecraft Server** → **Run workflow**
3. Get the Playit IP from the workflow logs and share it with players
4. Server runs for 5.5 hours, then auto-restarts

## Plugin auto-download

Edit `plugins.txt` in the repo root. Supports two entry types:

- **Modrinth ID** — `LJNGWSvH` → fetches latest release via Modrinth API
- **Direct URL** — `https://...` → downloaded as-is

Drop custom `.jar` files in the `plugins/` folder to override any auto-downloaded plugin.

## DiscordSRV setup

After adding `DISCORD_TOKEN` secret, you also need to set your Discord channel IDs.  
Edit `.github/workflows/main.yml` and change these two lines (around line 283):

```yaml
echo "Channels: {\"global\": \"1479509091155054712\"}" >> server/plugins/DiscordSRV/config.yml
echo "DiscordConsoleChannelId: \"1479518082295664730\"" >> server/plugins/DiscordSRV/config.yml
```

Replace the numbers with your actual Discord channel IDs:

1. Open Discord → Settings → Advanced → **Developer Mode** ON
2. Right-click the channel you want → **Copy Channel ID**
3. Paste it into the YAML, replacing the existing number (keep the quotes)
