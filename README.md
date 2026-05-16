\# yt-dlp Windows Setup Guide



A simple local setup to download YouTube videos using yt-dlp + FFmpeg on Windows.



\## Prerequisites

\- Windows 10/11

\- `winget` (comes with Windows 11; available for Win10 via App Installer)



\## Setup Steps



\### 1. Create a working directory

```cmd

mkdir C:\\ytdl

```



\### 2. Download yt-dlp

Go to https://github.com/yt-dlp/yt-dlp/releases and download `yt-dlp.exe`, then move it:

```cmd

move yt-dlp.exe C:\\ytdl

```



\### 3. Install FFmpeg (required for merging video+audio)

```cmd

winget install Gyan.FFmpeg

```

Restart your terminal after this so the PATH updates.



\### 4. Download a video

```cmd

cd C:\\ytdl

yt-dlp "https://www.youtube.com/watch?v=VIDEO\_ID"

```



\## Notes

\- yt-dlp auto-resumes interrupted downloads

\- Without FFmpeg, only low-quality single-format files are available

\- Output is saved as `.mkv` when video and audio are merged

\- To download only video (no playlist): add `--no-playlist` flag



\## Optional: Fix JS Runtime Warning

yt-dlp warns about missing Deno runtime. Install it with:

```cmd

winget install DenoLand.Deno

```

Then use: `yt-dlp --js-runtimes deno "URL"`

