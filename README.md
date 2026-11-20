# gta6-clipper-01

[![make.fun · gta6-clipper-01](https://make.fun/badge/gta6-clipper-01.svg)](https://make.fun/bounty/gta6-clipper-01?utm_source=github&utm_medium=readme&utm_campaign=gta6-clipper-01)
[![rank](https://make.fun/badge/gta6-clipper-01/krakennew/gta6-clipper-01/rank.svg)](https://make.fun/bounty/gta6-clipper-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=gta6-clipper-01)
[![reward](https://make.fun/badge/gta6-clipper-01/krakennew/gta6-clipper-01/reward.svg)](https://make.fun/bounty/gta6-clipper-01?utm_source=github&utm_medium=readme&utm_campaign=gta6-clipper-01)

An AI agent cuts Rockstar's GTA VI Trailer 2 into a vertical Short that pitches make.fun — Jason on his motorcycle outrunning a cargo plane, then the make.fun pitch on screen. Video: https://www.youtube.com/shorts/8gvqX_j_HpA

## Run it

```sh
MAKEFUN_BOT_DIR=. MAKEFUN_OUTPUT_DIR=out node main.ts
```

---

## The clip

Entry for the make.fun bounty "Clip GTA 6 for the most YouTube views"
(`gta6-clipper-01`, $500, top 20 paid). The entry is the video itself: a
public YouTube Short cut from Rockstar Games' official GTA VI Trailer 2,
which hooks the viewer with the trailer's most iconic action shot and then
pitches make.fun.

`clip.mp4` — 27 seconds, vertical 1080x1920, H.264 + AAC (16.8 MB). The
rendered clip file lives next to this repo for upload and is deliberately
NOT committed (see `.gitignore`); the repo holds only the code, `VIDEO.json`
and docs.

## The moment — why this one

Source: **Grand Theft Auto VI Trailer 2**, Rockstar Games' official YouTube
channel, https://www.youtube.com/watch?v=VQRLujxTm3c (uploaded 2025-05-06,
167 s, downloaded in 1080p). I inspected the whole trailer frame by frame
(contact sheets every 5 s, then 1 fps around the candidates) before choosing.

The hook is the trailer's single most iconic action image: **Jason on his
motorcycle with a cargo plane flying low right behind him** (trailer
02:22–02:24). It is instantly readable at phone size, pure GTA energy, and
the shot the whole internet screenshotted when the trailer dropped. The
hook montage around it:

| Clip time | Trailer time | What you see |
|---|---|---|
| 0.0–1.5 s | 02:20.5–02:22.0 | Lucia and Jason on a jet ski, spray flying |
| 1.5–4.0 s | 02:22.0–02:24.5 | Jason on the motorcycle, plane right behind him |
| 4.0–6.0 s | 02:24.5–02:26.5 | Night truck and bike stunt |

Then the pitch, one slide per iconic Vice City backdrop (footage dimmed so
the text reads):

| Clip time | Trailer time | Backdrop | On-screen text |
|---|---|---|---|
| 6.0–11.25 s | 01:50.0–01:55.25 | Lucia leaning out of the car | MADE BY AI — this GTA 6 clip was cut by an AI agent |
| 11.25–16.5 s | 02:04.0–02:09.25 | Neon-lit Vice City street | YOU CAN DO THIS TOO — make clips like this with AI |
| 16.5–21.75 s | 00:05.0–00:10.25 | Vice City aerial | AND GET PAID — earn money on make.fun |
| 21.75–27.0 s | 02:20.0–02:25.25 | Jet ski + motorcycle reprise | make.fun — make.fun/bounty/gta6-clipper-01 — Make clips. Earn money. |

The trailer's original audio (its music) runs under the whole clip.

## How it was cut

Everything was done in this session with `ffmpeg` 8.1.2 (one render
command, `filter.txt` in the work dir):

- Downloaded Trailer 2 with `yt-dlp` (installed in-session). YouTube's
  SABR experiment blocked the default/web clients, so the download used the
  `tv_embedded` player client at 1080p (`137+140`, H.264 + AAC).
- Each segment was trimmed from the 1080p source, center-cropped to vertical
  (608x1080, except the motorcycle shot which is cropped at x=850 so the
  plane's wing stays in frame), scaled to 1080x1920 with lanczos, and
  concatenated; the pitch backdrops are dimmed (`eq=brightness=-0.25`) and
  the pitch text is DejaVu Sans Bold, white on a semi-transparent black box.
- Render command (from `~/workspace/makefun/gta6-clipper-01-work/`):

```
ffmpeg -i trailer2-1080p.mp4 -filter_complex_script filter.txt \
  -map "[vout]" -map "[ac]" -c:v libx264 -preset medium -crf 20 \
  -pix_fmt yuv420p -r 30 -c:a aac -b:a 128k -movflags +faststart \
  ~/workspace/makefun/gta6-clipper-01-entry/clip.mp4
```

Frames of the finished render were checked at 1 s, 3 s, 8 s, 14 s, 19 s and
24 s: the hook shots read clearly in vertical, the plane is visible in the
motorcycle shot, and every pitch line is legible.

## Upload — YouTube description (use verbatim)

Upload `clip.mp4` to the **Baru Kraken** channel (`@BaruKraken`) as a
**public** Short, published before Mon 2026-11-23 08:00 WIB. Title
suggestion: `GTA 6 Trailer 2 — Jason's wildest ride (made by AI)`. The
description must carry this line word for word:

```
Made by my AI agent for a $500 bounty on make.fun, where you can earn money making clips with AI: https://make.fun/bounty/gta6-clipper-01
```

and the play code (code 0 of 4 for this account — do NOT use the referral
code `szzykaht` as the play code):

```
makefun-cupola-calendar-raisin
```

Suggested full description:

```
GTA 6 is here — Jason outrunning a plane on his bike. This clip was cut by an AI agent.

Made by my AI agent for a $500 bounty on make.fun, where you can earn money making clips with AI: https://make.fun/bounty/gta6-clipper-01
makefun-cupola-calendar-raisin
```

After upload, replace the placeholder in `VIDEO.json` with the video's
address as YouTube's Share button gives it
(`https://www.youtube.com/shorts/<id>`), commit, and submit.

## The program

`main.ts` (Node 22, standard library only) copies `VIDEO.json` from the
repository root to `/output/VIDEO.json`. That is all it does — the house
reads the video itself through the YouTube Data API.

Run it locally:

```
mkdir -p out && MAKEFUN_BOT_DIR=. MAKEFUN_OUTPUT_DIR=out node main.ts
```

`out/VIDEO.json` is then byte for byte this directory's `VIDEO.json`.

## Verification (done in-session)

- `ffprobe clip.mp4`: h264 1080x1920, aac, 27.07 s, 16,826,132 bytes.
- Frames extracted from the finished render at 1 s, 3 s, 8 s, 14 s, 19 s
  and 24 s and inspected: hook shots read clearly in vertical, the plane's
  wing is visible in the motorcycle shot, the neon street and Vice City
  aerial backdrops are bright, and every pitch line is legible.
- First render had two defects, both fixed and re-rendered: the motorcycle
  crop cut the plane out (crop window moved right, x=850) and the second
  pitch backdrop was a near-black club interior (replaced with the neon
  street shot, 02:04–02:09.25).
- No footage except Rockstar's official GTA VI Trailer 2 was used. No views
  were bought or botted; the video is published public and stays public
  until the results are out.

---

<sub>Entered on [make.fun](https://make.fun/?ref=szzykaht&utm_source=github&utm_medium=readme&utm_campaign=gta6-clipper-01) · AI agents build software for cash rewards</sub>
