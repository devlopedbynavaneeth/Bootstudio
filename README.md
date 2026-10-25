# Boot Animation Studio

A single-file web app for working with Android `bootanimation.zip` files. Open it in any modern browser, no install and no server needed.

It can:

- **Play** a `bootanimation.zip` the way a phone would.
- **Create** a `bootanimation.zip` from a built-in template or from a video.
- **Convert** a `bootanimation.zip` back into a video.

Everything runs locally in your browser. Your files are never uploaded anywhere.

---

## Quick start

1. Open `bootanimation-studio.html` in Chrome (or any modern browser).
2. Pick a tab: **Play**, **Create zip** or **Zip to video**.
3. Follow the steps below.

> The page loads one small library (JSZip) from a CDN, so you need an internet connection the first time you open it.

---

## Tabs

### 1. Play

Preview an existing `bootanimation.zip`.

| Control | What it does |
|---|---|
| **Open zip** | Choose a file, or drop it on the phone screen. |
| **Play / Pause / Restart** | Standard playback. |
| **Finish boot** | Ends looping parts, like the phone finishing its boot. |
| **Full screen** | Shows the animation full screen. |
| **Custom view size** | Set your own width and height for the playback screen, with a fit mode. **Use original** resets it. |

The player reads `desc.txt` for the size, frame rate and part rules, and follows the loop and pause settings of each part. It loads frames a few at a time, so large zips do not fill the phone's memory.

### 2. Create zip

Build a new `bootanimation.zip`. Choose a source at the top.

#### From template

Pick one of six templates. Each card shows a live preview, and the phone screen plays the selected one as an intro followed by a loop.

| Template | Look |
|---|---|
| **Wordmark** | Text fades in, an underline draws, a small dot glides along it. |
| **Pulse ring** | A ring draws itself, then pulses outward. |
| **Progress line** | A thin bar with a light segment sliding along it. |
| **Orbit** | Eight dots chase each other in a circle. |
| **Minimal** | Text fades in with a small breathing dot. |
| **Speed lines** | Horizontal lines sweep across before the text slides in. |

Customise:

- **Main text** (default `Velocity OS`) and **footer text** (default `by INFINITE`)
- **Font:** Modern, Geometric, Serif or Mono
- **Background:** black or white
- **Colour:** one accent colour only, from eight swatches or a custom picker

#### From video

Choose a video file. Frames are taken evenly from the whole video, in order. Choose how the video fits the screen: **Fill and crop**, **Fit with black bars** or **Stretch**.

#### Settings (both sources)

| Setting | Notes |
|---|---|
| **Screen size** | 720×1600, 1080×2400, 1080×1920, 1440×3200, or **Custom** (16 to 4096 px, rounded to even numbers). |
| **Frames per second** | 1 to 60 (default 30). |
| **How many parts** | 1 to 5. |
| **Frames per part** | Set how many frames each part gets. |
| **Plays per part** | `1` plays the part once. `0` loops it until the phone has finished booting. |

Defaults are `120 / 120 / 210` frames with plays `1 / 1 / 0`, which gives an intro, a middle part, and a seamless loop.

With a template, part0 is the intro and the last part is a seamless loop. If you use a single part, it plays the intro and then loops.

Press **Create zip**. A progress message shows the current frame, and **Cancel** stops the build.

### 3. Zip to video

Load a `bootanimation.zip` and record it as a video.

- Set the **video width and height** (it defaults to the zip's size) and a fit mode.
- The animation is recorded in real time, so a 15 second animation takes about 15 seconds. Keep the page open while it records.
- Looping parts play once.
- The result is an **MP4** where your browser supports it, otherwise **WebM**.

---

## What the generated zip contains

```
bootanimation.zip
├── desc.txt
├── part0/  boot_000.png … boot_119.png
├── part1/  boot_000.png … boot_119.png
└── part2/  boot_000.png … boot_209.png
```

Numbering restarts at `boot_000` in every part. The zip is stored **uncompressed**, as Android requires.

Example `desc.txt`:

```
720 1600 30
p 1 0 part0
p 1 0 part1
p 0 0 part2
```

- Line 1: `width height fps`
- Each following line: `p <plays> <pause> <folder>`, where `plays = 0` means loop until boot completes.

---

## Using the zip on a phone

On many Android devices the file goes to `/system/media/bootanimation.zip` with permissions `644`. This normally needs root access or a custom ROM, and the exact path varies by device. Make a backup of the original first.

---

## Downloads and sharing

- Inside Claude, files are saved through Claude's save prompt.
- Outside Claude (for example, the standalone HTML file opened in Chrome), the app uses a normal browser download.

---

## Limitations

- **Samsung `.qmg` files are not supported.** QMG is a proprietary Samsung format and cannot be decoded in the browser. Convert it to PNG frames or a video with a desktop tool, then use **Create zip**. Exporting to QMG is also not possible.
- **Memory.** Large sizes with many frames use a lot of memory. `720×1600` is the safe choice on a phone; `1080×2400` with hundreds of frames may fail.
- **Real-time video export.** Zip to video records live, so it takes as long as the animation lasts, and needs the page to stay open.
- **Browser support.** Video export needs MediaRecorder (Chrome, Edge and Safari support it). Full-screen mode may not work on every mobile browser.
- **Colour options.** The `desc.txt` background colour field is not read; the player background is always black.

---

## Hosting it yourself

The app is one HTML file, so any static host works. Free options include Netlify Drop, Cloudflare Pages and GitHub Pages. Upload `bootanimation-studio.html` (rename it to `index.html` if the host expects that).

---

## Design notes

- The interface uses a single accent colour on neutral greys, with light and dark modes that follow the device setting.
- The templates use one accent colour on a black or white background.
- The favicon is a play button inside a ring, embedded in the page.
