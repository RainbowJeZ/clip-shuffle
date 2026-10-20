# ClipShuffle

Plays short, random moments from a YouTube playlist, cutting between videos for as long as you leave it running. Made for a TV in the background at a party.

- Picks a random video and a random point in it, plays it for a set time, then crossfades to another video.
- Every clip is played once before any repeat, and the same video never plays twice in a row.
- Skips the first and last few seconds of each video, so you don't see studio logos and end screens.
- Plays muted, so your own music stays in charge (and browsers allow muted video to start by itself).
- You can add hand-picked scenes with exact start and end times.
- Skips any video whose owner blocks it from playing outside YouTube.

## Using it

Open the site and paste a playlist link, or go straight to an address like this:

```
https://<your-username>.github.io/clip-shuffle/?list=PLAYLIST_ID&clip=25
```

The playlist must be **Public** or **Unlisted**. Private playlists can't be read.

| Address setting | What it does | Default |
|---|---|---|
| `list` | YouTube playlist ID or full link | — |
| `v` | Extra video IDs, separated by commas | — |
| `preset` | Loads `presets/<name>.json` | — |
| `clip` | Seconds per clip (5–300) | 25 |
| `skip` | Seconds to avoid at the start and end of each video | 8 |
| `fade` | Crossfade length in milliseconds (0 = hard cut) | 700 |
| `titles` | `0` hides the title shown at the start of each clip | 1 |

### Controls

| Key | Action |
|---|---|
| Tap / click / `F` | Full screen |
| `→` or `N` | Next clip |
| `Space` | Pause / carry on |
| `H` | Show / hide the info panel |

## Presets: one file per party or theme

Copy `presets/example.json`, give it a name such as `presets/halloween.json`, and open `?preset=halloween`.

```json
{
  "name": "Halloween",
  "playlist": "https://www.youtube.com/playlist?list=...",
  "clipSeconds": 25,
  "skipSeconds": 8,
  "showTitles": true,
  "randomClips": true,
  "scenes": [
    { "video": "https://www.youtube.com/watch?v=...", "start": "1:05", "end": "1:30", "label": "Optional caption" }
  ]
}
```

- `scenes` are mixed into the rotation alongside the random moments. Times can be seconds (`65`) or `m:ss` (`1:05`). If `end` is left out, the scene plays for `clipSeconds`.
- `"randomClips": false` plays only your chosen scenes.
- Anything in the address (for example `&clip=15`) overrides the preset.

## Publishing on GitHub Pages

1. Create a repository called `clip-shuffle` and upload `index.html`, `README.md` and the `presets` folder.
2. In the repository, go to **Settings → Pages**, set **Source** to *Deploy from a branch*, pick `main` and `/ (root)`, then **Save**.
3. After a minute or two the site is live at `https://<your-username>.github.io/clip-shuffle/`.

To test on your own computer, don't open `index.html` by double-clicking it, because YouTube refuses to play from a local file. Run a small web server in the folder instead:

```
python -m http.server 8000
```

Then open http://localhost:8000.

## Running it on a TV

**Windows tablet or laptop:** open the address in Chrome or Edge, tap the screen to go full screen, and connect to the TV by HDMI or wireless casting. Set the device not to sleep while plugged in. The page also asks the browser to keep the screen awake.

**Raspberry Pi 4 or 5:** connect it to the TV with HDMI and open the address in Chromium in kiosk mode:

```
chromium-browser --kiosk --noerrdialogs --disable-infobars --autoplay-policy=no-user-gesture-required "https://<your-username>.github.io/clip-shuffle/?preset=halloween"
```

On newer Raspberry Pi OS the command may be `chromium` rather than `chromium-browser`. To start it automatically when the Pi boots, add that line (ending with `&`) to `~/.config/labwc/autostart`. On a Pi 4, if playback stutters, set the TV output to 1080p rather than 4K.
