# Spotify Widget

An OBS-ready widget that displays your currently playing Spotify track. Uses the Spotify Web API to fetch your real-time listening status and renders a clean overlay perfect for streaming.

## Features

- Shows album art, track name, artist
- Live progress bar (current position / duration)
- Paused / playing state indicator
- Auto-refreshes every 5 seconds
- Transparent background by default (OBS-ready)
- Customizable via URL query parameters
- Self-hostable via Docker

## Setup

1. Create an app at [Spotify Developer Dashboard](https://developer.spotify.com/dashboard)
2. Add your deployment URL as a Redirect URI (e.g. `http://localhost:3000/api/auth/callback`)
3. Copy `.env.example` to `.env.local` and fill in your credentials
4. Start the dev server, visit `/api/auth` to authorize, then paste the refresh token into `.env.local`
5. Restart the server

## Self-Hosting

### Docker

```bash
docker build -t spotify-widget .
docker run -p 3000:3000 \
  -e SPOTIFY_CLIENT_ID=your_id \
  -e SPOTIFY_CLIENT_SECRET=your_secret \
  -e SPOTIFY_REDIRECT_URI=http://your-domain:3000/api/auth/callback \
  -e SPOTIFY_REFRESH_TOKEN=your_token \
  spotify-widget
```

### Docker Compose

```yaml
services:
  spotify-widget:
    image: ghcr.io/rnnve/spotify-widget.self:latest
    ports:
      - "3000:3000"
    environment:
      - SPOTIFY_CLIENT_ID=your_id
      - SPOTIFY_CLIENT_SECRET=your_secret
      - SPOTIFY_REDIRECT_URI=http://your-domain:3000/api/auth/callback
      - SPOTIFY_REFRESH_TOKEN=your_token
    restart: unless-stopped
```

A pre-built image is available on [GitHub Container Registry](https://github.com/rnnve/spotify-widget.self/pkgs/container/spotify-widget.self).

## OBS Integration

Add a **Browser Source** in OBS with:
- URL: `http://localhost:3000/widget` (or your deployment URL)
- Width: 400, Height: 120
- Enable "Refresh browser when scene becomes active"

## Widget Customization

Append URL query parameters to `/widget` to customize the appearance:

| Parameter | Default | Description |
|-----------|---------|-------------|
| `bg` | transparent | Background color (any CSS color or `transparent`) |
| `color` | #ffffff | Text color |
| `font` | system-ui | Font family |
| `progress` | bar | Show progress bar: `bar`, `none` |
| `size` | md | Text size: `sm`, `md`, `lg` |

### Examples

```
/widget?bg=#000000&color=#1db954
/widget?bg=rgba(0,0,0,0.5)&font=monospace
/widget?progress=none
/widget?size=lg&bg=#1a1a2e&color=#e94560
```

## Tech

- [Next.js](https://nextjs.org) (App Router)
- [Tailwind CSS](https://tailwindcss.com)
- [Spotify Web API](https://developer.spotify.com/documentation/web-api)
