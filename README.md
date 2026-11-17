# AI Video Ads — Portfolio

A one-page portfolio site for GitHub Pages. No build step: it's just `index.html`, an `img/` folder of cover images and a `videos/` folder.

## 1. Add your details

Open `index.html`, scroll near the bottom and edit the `CONFIG` block:

```js
const CONFIG = {
  name:     "Your Name",
  email:    "you@gmail.com",
  whatsapp: "639171234567",   // digits only, with country code
  discord:  "yourname",
  resume:   "resume.pdf",     // put the PDF next to index.html
  location: "Philippines (GMT+8)",
  ...
};
```

Any field left as `""` is hidden on the site.

## 2. Put it on GitHub Pages

The github.com upload page only accepts files up to 25 MB, and three of the drama videos are bigger (33–45 MB). GitHub Desktop handles files up to 100 MB, so use it:

1. Install **GitHub Desktop** (desktop.github.com) and sign in to your GitHub account.
2. **File → New repository**. Name it `yourusername.github.io` for a site at `https://yourusername.github.io`, or any name (like `portfolio`) for `https://yourusername.github.io/portfolio`. Pick a folder on your computer and click **Create repository**.
3. Copy everything from this portfolio folder (`index.html`, `README.md`, `.nojekyll`, `img/`, `videos/`) into the new repository folder.
4. Back in GitHub Desktop, type a summary like "Portfolio site", click **Commit to main**, then **Publish repository**. Untick "Keep this code private" so the site can go live, then click **Publish repository**.
5. On github.com, open the repo and go to **Settings → Pages**. Under "Build and deployment", set Source to **Deploy from a branch**, Branch to **main** and folder to **/ (root)**, then **Save**.
6. Wait 1–2 minutes and open the link GitHub shows at the top of the Pages settings.

## 3. Add or swap an ad

1. Put the video in `videos/` (keep each file under 100 MB — GitHub's limit) and a cover image in `img/` with the same name, e.g. `videos/new-ad.mp4` + `img/new-ad.jpg`.
2. In `index.html`, find `const VIDEOS = [` and copy one line:

```js
{file:"new-ad", style:"ugc", lang:"EN", len:"0:45", title:"My new ad", desc:"One line about it.", hover:5},
```

`style` is one of: `drama`, `pixar`, `anim`, `ugc`, `vsl`, `podcast`. `hover` is the second the preview starts from when someone hovers the card.

To shrink a big video before uploading:

```
ffmpeg -i input.mp4 -vf scale=720:-2 -c:v libx264 -crf 25 -preset veryfast -c:a aac -b:a 96k -movflags +faststart videos/new-ad.mp4
```

## Long videos (streaming chunks)

Four long ads (`pixar-wrinkles`, `drama-date`, `drama-long`, `drama-christmas`) are stored as small streaming chunks in `videos/<name>/index.m3u8` + `s000.ts`, `s001.ts`… instead of one big .mp4. Their lines in `VIDEOS` have `hls:true`. The site plays them with the bundled `js/hls.min.js` (Chrome, Edge, Firefox) or natively (Safari, iPhone).

To make your own long video into chunks:

```
mkdir videos/my-ad
ffmpeg -i my-ad.mp4 -c copy -f hls -hls_time 6 -hls_playlist_type vod -hls_segment_filename "videos/my-ad/s%03d.ts" videos/my-ad/index.m3u8
```

Then add it to `VIDEOS` with `hls:true`. Short videos can stay as plain .mp4 files.
