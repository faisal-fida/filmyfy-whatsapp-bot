# filmyfy-whatsapp-bot

FilmyFy is a WhatsApp userbot (Node.js, Baileys) with admin tools, media converters and web lookups, derived from the WhatsAsena project.

## Credits and license

This is a fork of the WhatsAsena bot (also published as WhatsAsenaDuplicated) by Yusuf Usta and phaticusthiccy (`AsenaDev`); the idea and base code come from that project. It is licensed under the GNU General Public License v3.0 (see `LICENSE`), as the upstream is. Source files keep their original "Copyright (C) 2020 Yusuf Usta" GPL headers. Changes in this fork are also GPL-3.0.

## Status

Old project (2020 to 2021 era dependencies, for example Baileys 3.4.1, which the current WhatsApp Web protocol no longer supports), so the bot probably does not connect today without upgrades. Some code paths are inherited upstream quirks: `bot.js` and `plugins/textmaker.js` return early unless the user's home directory matches an encoded path, and `plugins/Eva Artificial Intelligence.js` expects the folder name to be `WhatsAsena...Duplicated`. The "Setup" section of the old README pointed to upstream deploy buttons and wiki that are not part of this repo. Using a userbot can get a WhatsApp account banned. No tests. Only the English language file (`language/EN.json`) is included, although `config.js` accepts other `LANGUAGE` values.

## Features

Plugins under `plugins/` (each registers commands through `events.js`):

- Group admin: ban, promote, demote, mute, tag all, tag admins, greetings and goodbye messages, filters, notes, AFK message.
- Media: sticker maker and converters, ffmpeg audio and video edits, text-maker images, carbon code images, OCR (tesseract), remove.bg background removal, wallpaper, screenshots, TikTok and social media downloads.
- Lookups and utilities: lyrics, currency, weather, Wikipedia, translation and text-to-speech, whois, Spotify and web scrapers, system stats.
- AI helpers: DeepAI, "Eva" chat, Lydia chatbot (CoffeeHouse), voice to text (Wit.ai).
- Admin of the bot itself: plugin install and remove, Heroku config and updater.
- Storage through Sequelize (SQLite by default, PostgreSQL via `DATABASE_URL`).

## Tech stack

Node.js, `@adiwajshing/baileys`, Sequelize (SQLite or PostgreSQL), ffmpeg, Heroku (optional, `app.json`, `heroku.yml`, `whatsasena/Dockerfile`).

## Structure

```
bot.js, config.js, events.js, helpers.js, language.js   core
plugins/            command plugins; plugins/sql/ database models
whatsasena/         message wrapper classes and Dockerfile
language/EN.json    texts
app.json, heroku.yml  Heroku deploy config
qr.js               QR session helper
```

## Setup

Requirements: Node.js (an old LTS, matching the dependencies), ffmpeg, and for some plugins tesseract.

```bash
npm install
cp /dev/null config.env   # then add variables below, one KEY=value per line
node bot.js
```

`config.js` loads `config.env` if it exists. Main environment variables:

| Name | Purpose |
|------|---------|
| `ASENA_SESSION` | saved WhatsApp session string (from `qr.js`) |
| `LANGUAGE` | bot language code (only `EN` file is included) |
| `WORK_TYPE` | `private` (default) or `public` |
| `SUDO` | extra user IDs allowed to use the bot |
| `HANDLERS` | command prefix regex, default `^[.!;]` |
| `DATABASE_URL` | PostgreSQL URL; unset uses local `whatsasena.db` |
| `HEROKU`, `HEROKU_API_KEY`, `HEROKU_APP_NAME` | Heroku updater and config plugins |
| `REMOVE_BG_API_KEY` | remove.bg plugin |
| `COFFEEHOUSE_API_KEY` | Lydia chatbot |
| `WITAI_API_KEY` | voice to text and Eva (Wit.ai token) |
| `SPOTIFY_CLIENT_ID`, `SPOTIFY_CLIENT_SECRET` | Spotify search plugin |
| `ALIVE_MESSAGE`, `BAN_MESSAGE`, `ADD_MESSAGE` and other `*_MESSAGE` | custom reply texts |
| `NO_LOG`, `FULL_EVA`, `SEND_READ`, `NO_ONLINE`, `DEBUG`, `ANTI_LINK`, `AUTO_BIO` | feature switches |

Never commit `config.env`, `whatsasena.db` or `session.asena` (they are in `.gitignore`).

## Usage

Start the bot, scan the QR code with the WhatsApp account that will run it, then send a command from that account, for example `.alive` or `.help` (prefix set by `HANDLERS`).
