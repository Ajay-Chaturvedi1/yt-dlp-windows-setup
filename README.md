# yt-dlp Windows Setup Guide

A simple local setup to download YouTube videos using **yt-dlp + FFmpeg** on Windows.

---

## Prerequisites

- Windows 10/11
- `winget` (comes with Windows 11; available for Win10 via App Installer)
- Internet connection

---

## Setup Steps

### Step 1 — Create a Working Directory

```cmd
mkdir C:\ytdl
cd C:\ytdl
```

### Step 2 — Download yt-dlp

Go to [https://github.com/yt-dlp/yt-dlp/releases](https://github.com/yt-dlp/yt-dlp/releases) and download `yt-dlp.exe`, then move it to your working directory:

```cmd
move C:\Users\<YourUsername>\Downloads\yt-dlp.exe C:\ytdl
```

### Step 3 — Install FFmpeg

```cmd
winget install Gyan.FFmpeg
```

> ⚠️ **IMPORTANT: Restart your CMD terminal after this step before moving forward.**

#### Why You Must Restart CMD After Installing FFmpeg

When FFmpeg is installed via `winget`, Windows automatically adds its location to the **PATH environment variable**.

The **PATH** is a list of folders that Windows checks when you type a command — so when you type `ffmpeg`, Windows knows where to find it.

**The problem:** Your current CMD session loaded the old PATH when it first opened — **before FFmpeg was installed**. So even though FFmpeg is now installed, your current terminal does not know it exists yet.

| Situation | Result |
|---|---|
| ❌ Run yt-dlp **without** restarting CMD | `WARNING: ffmpeg not found` — downloads low quality |
| ✅ Run yt-dlp **after** restarting CMD | FFmpeg is detected — downloads best quality and merges |

**How to restart properly:**
1. Close your current CMD window completely
2. Open a **new** CMD window (search `cmd` in Start Menu)
3. Navigate back: `cd C:\ytdl`
4. Now run yt-dlp — FFmpeg will be detected automatically

> Think of it like this: PATH is a **map** your terminal uses to find tools. Installing FFmpeg adds it to the map, but your old terminal is still using the **old map without FFmpeg on it**. Opening a new terminal loads the **updated map**.

### Step 4 — Download a Video

```cmd
cd C:\ytdl
yt-dlp "https://www.youtube.com/watch?v=VIDEO_ID"
```

---

## Why FFmpeg is Required

YouTube stores **video and audio as two separate files** on their servers.  
When you download a video, yt-dlp fetches them separately and needs **FFmpeg to merge them into one complete file**.

### What Happens Without FFmpeg vs With FFmpeg

| Situation | Result |
|---|---|
| ❌ No FFmpeg installed | yt-dlp downloads a **single low-quality file** (360p or 480p max) that already has video+audio combined |
| ✅ FFmpeg installed | yt-dlp downloads the **best video** (1080p/4K) + **best audio** separately, then merges them into one high-quality file |

### Real Example from This Setup

**Without FFmpeg** (first attempt — poor quality):
```
WARNING: ffmpeg not found.
[info] Downloading 1 format(s): 18    ← format 18 = only 360p
```

**With FFmpeg** (successful attempt — best quality):
```
[info] Downloading 1 format(s): 137+251    ← 137 = 1080p video, 251 = best audio
[Merger] Merging formats into "video.mkv" ← FFmpeg merges them into one file
```

### In Simple Words

> Think of it like this — YouTube sends the **picture** and **sound** in two separate envelopes.  
> **FFmpeg** is the tool that puts them together into one complete, high-quality video.  
> Without it, you only get a low-resolution version where YouTube already packed both into one (but at poor quality).

---

## Notes

- yt-dlp **auto-resumes** interrupted downloads — just re-run the same command
- Output is saved as `.mkv` when video and audio are merged by FFmpeg
- To download a single video (skip playlist): add `--no-playlist` flag
- Downloads are saved in the directory where you run the command (`C:\ytdl`)

---

## Optional: Fix JS Runtime Warning

yt-dlp may show this warning:

```
WARNING: No supported JavaScript runtime could be found. Only deno is enabled by default
```

To fix it, install Deno:

```cmd
winget install DenoLand.Deno
```

Then use:

```cmd
yt-dlp --js-runtimes deno "https://www.youtube.com/watch?v=VIDEO_ID"
```

---

## Folder Structure

```
C:\ytdl\
│
├── yt-dlp.exe          ← the downloader
└── <downloaded videos> ← saved here automatically
```

---

## Resources

- [yt-dlp GitHub](https://github.com/yt-dlp/yt-dlp)
- [FFmpeg Official Site](https://ffmpeg.org/)
- [yt-dlp JS Runtime Guide](https://github.com/yt-dlp/yt-dlp/wiki/EJS)
