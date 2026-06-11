# Free Fire Sensitivity Bot

A Telegram bot that generates custom Free Fire sensitivity settings based on the user's device, playstyle, and FPS.

## Run & Operate

- **Bot workflow**: `python bot/main.py` — runs the Telegram bot continuously (managed via "Free Fire Sensitivity Bot" workflow)
- Required secret: `TELEGRAM_BOT_TOKEN` — get from @BotFather on Telegram

## Stack

- Python 3.11
- python-telegram-bot v22 (async, polling)
- pnpm workspaces, Node.js 24 (pre-existing monorepo scaffolding)

## Where things live

- `bot/main.py` — entire bot: sensitivity logic, conversation handler, inline keyboard flow

## Architecture decisions

- Conversation is managed as a 3-step ConversationHandler (Device → Playstyle → FPS).
- Sensitivity values are seeded from per-device/per-playstyle base tables and then adjusted by an FPS multiplier (60 FPS = 1.0×, 90 FPS = 0.90×, 120 FPS = 0.82×) plus a small random jitter (±2) for personalization.
- `per_message=True` on ConversationHandler so each CallbackQuery is tracked independently — avoids PTBUserWarning.
- All state lives in `context.user_data`; no database needed.

## Product

Users send `/start`, pick their device (iPhone / Android / Tablet), playstyle (One Tap / Drag Headshot / Balanced / Rush / Sniper), and FPS (60 / 90 / 120). The bot replies with a formatted sensitivity card and a "Generate Again" button.

## User preferences

_Populate as you build — explicit user instructions worth remembering across sessions._

## Gotchas

- `TELEGRAM_BOT_TOKEN` must be set in Replit Secrets before starting the bot.
- The bot uses long-polling — it does not expose any HTTP port.
- Changing base sensitivity tables requires restarting the workflow.
