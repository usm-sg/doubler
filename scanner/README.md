# Doubler nightly scan

A free GitHub Actions job that runs after every US market close (about 6:30am
Singapore time, Tuesday to Saturday). It checks your watchlist plus every S&P 500
stock with the app's READY rules and saves the results to `data/scan.json`,
which the app's **Nightly** tab reads. It can also send the READY list to Telegram.

## Files

| File | What it does |
|---|---|
| `.github/workflows/nightly.yml` | The schedule and steps GitHub runs |
| `scanner/nightly_scan.py` | The scan |
| `scanner/doubler_backtest.py` | Shared rule code (same as the backtest) |
| `scanner/watchlist.txt` | Your scan watchlist; edit it on GitHub any time |
| `data/scan.json` | Created by the job; read by the app |
| `data/ready_history.csv` | Created by the job; every READY stock, every day |

## One-time setup

1. Copy everything into your Doubler repo (keep the folder names, including
   the `.github` folder, which starts with a dot).
2. On GitHub: **Settings → Actions → General → Workflow permissions** →
   choose **Read and write permissions** → Save.
3. **Actions** tab → **Doubler nightly scan** → **Run workflow**. It takes
   about 3-6 minutes. A green tick means it worked, and `data/scan.json`
   appears in the repo.
4. Open the app → **Nightly** tab → **Refresh**.

## Optional: Telegram message each morning

1. In Telegram, message **@BotFather** → `/newbot` → follow the steps. Copy the
   bot token it gives you.
2. Send any message to your new bot, then open
   `https://api.telegram.org/bot<TOKEN>/getUpdates` in a browser and copy the
   number after `"chat":{"id":`.
3. Repo **Settings → Secrets and variables → Actions**:
   - **Secrets** tab → New secret `TELEGRAM_BOT_TOKEN` = the token
   - **Secrets** tab → New secret `TELEGRAM_CHAT_ID` = the chat id
   - **Variables** tab (optional) → `APP_URL` = your app link, added to the message

## Notes

- GitHub may start scheduled runs 5-30 minutes late.
- GitHub pauses scheduled jobs in public repos after 60 days without repo
  activity. If that happens, the Actions tab shows a button to re-enable it.
- To scan only your watchlist, remove `--sp500` from the last line of the
  "Scan watchlist + S&P 500" step in `nightly.yml`.
- Prices come from Yahoo Finance via yfinance, for personal use. A public repo
  makes `data/scan.json` visible to anyone.
- Not financial advice.
