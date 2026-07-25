# byway-player

A single static page that plays a YouTube Short inside the ByWay / Scentry iOS app.

## Why it exists

iOS reserves the `https` scheme, so a Capacitor app is always served from
`capacitor://localhost`. WKWebView sends no `Referer` header for a custom
scheme, and YouTube refuses to embed a video without one — it returns
**error 153, "embedder identity missing referrer"**.

The app therefore embeds *this page* instead of embedding YouTube directly.
Served from `github.io` over https, it gives the YouTube iframe a real referrer,
which YouTube accepts.

The speaker button lives in this page rather than in the app because browsers
grant audio permission to the frame the user actually tapped — a tap in the
parent document does not activate a cross-origin child frame.

## Usage

```
https://<user>.github.io/byway-player/player.html?v=VIDEO_ID
```

- `v` — YouTube video id (required)
- `mute=0` — start unmuted (browsers will usually still block this)

It posts `{ source: 'byway-player', event: 'ready' | 'playing' | 'error' | 'sound' }`
to the parent window, and accepts `{ target: 'byway-player', action: 'pause' | 'play' | 'mute' }`.

No API keys, no analytics, no third-party code beyond YouTube's own iframe API.
