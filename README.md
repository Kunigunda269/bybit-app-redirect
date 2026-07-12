# bybit-app-redirect

A tiny static redirect page that opens the **Bybit mobile app** on a specific
trading pair from a plain `https://` link — so a Telegram inline button (which
cannot carry a `bybit://` deep link) can still land the user inside the app.

## How it works

`index.html` reads a target from the `?u=` query parameter, validates it, and
builds an Android `intent://` URL for `package=com.bybit.app` with a web
fallback. On desktop / unsupported devices it degrades to the Bybit website.

Only `trade/<category>/<SYMBOL>` targets are accepted (with an optional
`interval`), everything else is rejected — no open redirect.

## Usage

Host `index.html` anywhere static (GitHub Pages, Cloudflare Pages, etc.) and
point links at it:

```
https://<your-host>/?u=bybit://trade/usdt/BTCUSDT?interval=5
https://<your-host>/?u=https://www.bybit.com/trade/usdt/ETHUSDT
```

The page shows fallback links ("Open in Bybit app" / "Open website instead")
if the automatic redirect doesn't fire.

## Notes

- No dependencies, no build step, no tracking — a single HTML file.
- Android App Links verify `app.bybit.com` (not `www`); the intent URL uses
  that host accordingly.
