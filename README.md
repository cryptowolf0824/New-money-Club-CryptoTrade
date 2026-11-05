# WEEX public strategy performance · New money club

Read-only static site for the single WEEX account managed by the WEEX trader bot.

- **Public display name:** New money club
- **Ordinal:** ① / #1
- **Exchange:** WEEX
- **Refresh:** daily (Asia/Shanghai) — overwrite `data.json` only
- **Desensitization:** display name **New money club** is allowed on this page; still **no UID / API keys / env names / MCP ids**
- **Target GitHub repo:** `cryptowolf0824/New-money-Club-CryptoTrade` → GitHub Pages from `main` root

Sibling Bitget multi-account page stays at `wolf-tranding` / `/workspace/public-strategy-site/` — do not merge the two.

## Files

| File | Role |
| --- | --- |
| `index.html` | UI (KPI + 合约/现货持仓卡片 + 分户表 + 历史策略) |
| `data.json` | Scrubbed KPI / account / positions / strategies |
| `refresh-public-data.sh` | Validate (or copy-from-SRC) `data.json` |
| `baseline.json` | Optional principal/onboard baseline (ops only; not required on Pages) |
| `README.md` | This file |

## What’s on the page

| Block | Content |
| --- | --- |
| KPI | Equity, principal, net PnL, return %, strategy count, strategy net U |
| 合约持仓 / 现货持仓 | Same-size cards under KPI; futures empty →「暂无持仓」; spot empty → blank; **strategy holdings only** |
| Per-account | ① + display name **New money club**, onboard, equity, principal, PnL, return %, W/L |
| History | Open/close time (Asia/Shanghai), symbol, side, prices, PnL |
| Notes | Public methodology + disclaimer |

Null `principal` / `net_pnl` / `return_pct` / `onboard` render as **待定** / **—** (no invented numbers in the UI).

## Local preview

```bash
cd /workspace/public-strategy-site-weex
python3 -m http.server 8766
# open http://127.0.0.1:8766/
```

`file://` will not load `data.json` (browser CORS); use any static HTTP server.

## Refresh data

```bash
/workspace/public-strategy-site-weex/refresh-public-data.sh
```

Optional: copy a corrected WEEX snapshot into place, then validate:

```bash
SRC_JSON=/path/to/weex-public-snapshot.json \
  /workspace/public-strategy-site-weex/refresh-public-data.sh
```

The script **does not** call WEEX APIs and **does not** invent equity/principal. Live pull is owned by the WEEX trader bot; this folder only hosts the scrubbed public JSON.

## Deploy — GitHub Pages

1. Create empty public repo `cryptowolf0824/New-money-Club-CryptoTrade`.
2. Push **only** site root contents: `index.html`, `data.json`, `README.md` (optional: `refresh-public-data.sh`).
3. Settings → Pages → Deploy from branch **main** / **root**.
4. URL shape: `https://cryptowolf0824.github.io/New-money-Club-CryptoTrade/`

Cloud Agent / ops with `gh` auth can push `main`; this workspace may not have `gh` logged in.

### Nightly pattern

1. WEEX trader bot writes corrected scrubbed `data.json` (or a snapshot path).
2. Run `refresh-public-data.sh` (copy + validate).
3. Commit/push `data.json` to `New-money-Club-CryptoTrade` main (Pages redeploy).
4. Visitors fetch with `cache: 'no-store'`.

Never commit `.env`, API keys, MCP tokens, UIDs, or internal account maps.

## Out of scope

- Bitget ①–⑮ site (`wolf-tranding`)
- Live order placement
- Creating Cursor routines (parent / ops)
- Inventing principal or equity figures in this folder
