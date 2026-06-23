# YouTube Custom Downloader & Analyzer

An interactive command-line tool to search YouTube, scan video streams frame-by-frame for visual criteria, and download or harvest trimmed video clips.

This customized package dynamically switches output paths depending on the host operating system.

---

## 📂 Cross-Platform Destination Folder Configuration
The script dynamically configures where your downloads, `registry.json`, and Word document are saved:
* **Windows Devices:** Automatically saved inside your user profile's Downloads folder in a directory named **`hafiz`**:
  * **Default Path:** `C:\Users\<username>\Downloads\hafiz`
  * **Custom Subfolders:** If you input a custom name (e.g. `'talks'`), it downloads to `C:\Users\<username>\Downloads\hafiz\talks`.
* **macOS Devices:** Automatically saved directly onto your Desktop in a folder called **`Vibe_Download`**:
  * **Default Path:** `/Users/<username>/Desktop/Vibe_Download` (or `~/Desktop/Vibe_Download`)
  * **Custom Subfolders:** If you input a custom name (e.g. `'talks'`), it downloads to `~/Desktop/Vibe_Download/talks`.
* **Linux/Other Devices:** Saved in a folder inside the current working directory.

---

## Features

* **Terminal Q&A Wizard:** Simply run the script to configure parameters interactively.
* **Metadata & Link Harvester Mode:** Choose to perform visual scans and log the timestamps/links in `registry.json` and `youtube_video_links.docx` *without* downloading large media files.
* **Flexible Visual Filters:**
  * **Single Speaker:** Checks for steady close-ups containing exactly 1 face.
  * **Two Speakers:** Validates dialogue scenes containing exactly 2 faces.
  * **Any Face:** Checks for frames containing at least 1 face.
  * **Generic:** Bypasses face-checking, capturing the first stable uncut shot.
  * **Others (Custom Builder):** Prompts you to input custom face count (e.g. `3`, `>=1`), face presence ratio (e.g. `0.7`), and minimum face size constraints.
* **Non-Blocking Timed Buffer Warnings:** Warns you if a video is streaming slowly and offers to skip. If you don't respond, it auto-continues scanning.
* **Perfect MP4 Remuxing:** Forces `yt-dlp` to remux final merged downloads directly to standard `.mp4` container files (no `.mkv` or `.webm` suffix mismatches).
* **Professional Unicode Progress Bars:** Beautiful real-time progress indicators for clip scanning and analysis.

---

## Prerequisites

1. **Python 3.8+**
2. **FFmpeg:** Required by `yt-dlp` to merge and trim video/audio streams.
   * **macOS:** Install via [Homebrew](https://brew.sh):
     ```bash
     # If you already have Homebrew installed:
     brew install ffmpeg

     # If you don't have Homebrew installed, run this first to install Homebrew:
     /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

     # Then install FFmpeg:
     brew install ffmpeg
     ```
   * **Linux (Ubuntu/Debian):**
     ```bash
     sudo apt update && sudo apt install ffmpeg
     ```
   * **Windows:**
 ## Installing FFmpeg on Windows via PowerShell

Follow these steps to automatically download, extract, and configure FFmpeg on your Windows system using PowerShell.

### ⚠️ Prerequisites
* You **must** run PowerShell as an **Administrator** because the script modifies files directly on the `C:\` drive.
* After running the script, you will need to restart your terminal or command prompt for the environment changes to take effect.

### Installation Script

Copy and paste the entire block below into your Administrator PowerShell window:

```powershell
# 1. Download the latest FFmpeg release essentials build
curl -L -o ffmpeg.zip [https://www.gyan.dev/ffmpeg/builds/ffmpeg-release-essentials.zip](https://www.gyan.dev/ffmpeg/builds/ffmpeg-release-essentials.zip)

# 2. Extract it to a temporary folder
Expand-Archive ffmpeg.zip -DestinationPath C:\ffmpeg_temp -Force

# 3. Move the actual inner contents to C:\ffmpeg and clean up
Move-Item C:\ffmpeg_temp\ffmpeg-*-essentials_build C:\ffmpeg -Force
Remove-Item C:\ffmpeg_temp -Recurse -Force
Remove-Item ffmpeg.zip -Force

# 4. Add FFmpeg to User PATH safely (prevents duplicate entries)
$currentPath = [System.Environment]::GetEnvironmentVariable("PATH", "User")
if ($currentPath -notlike "*C:\ffmpeg\bin*") {
    [System.Environment]::SetEnvironmentVariable("PATH", $currentPath + ";C:\ffmpeg\bin", "User")
    Write-Host "FFmpeg added to PATH successfully!" -ForegroundColor Green
} else {
    Write-Host "FFmpeg is already in your PATH." -ForegroundColor Yellow
}
     ```
     *(Note: Restart your PowerShell or Command Prompt terminal after running these commands to load the new PATH variable).*

---
```

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/hafizkhan902/custom_youtube_downloader.git
   cd custom_youtube_downloader
   

2. Install python dependencies:
   ```bash
   pip install -r requirements.txt
   ```

---

## Usage

### 1. Interactive Q&A Wizard (Recommended)
Launch the script without arguments to start the interactive wizard:
```bash
python3 youtube_clip_downloader.py
```

### 2. Command-Line Arguments (Automated Mode)
Bypass the wizard and run directly by specifying CLI options:

```bash
python3 youtube_clip_downloader.py --query "TED talk" --count 5 --mode single-speaker --min-len 20 --max-len 30 --cc y -o my_downloads
```

#### CLI Reference
* `--query`, `-q`: The YouTube search term.
* `--cc`: Creative Commons filter (`y` or `n`).
* `--count`, `-n`: Target number of clips to find.
* `--min-len`: Minimum duration of the trimmed clip in seconds (default: `22`).
* `--max-len`: Maximum duration of the trimmed clip in seconds (default: `30`).
* `--download` / `--no-download`: Enable video downloading or run in metadata harvester mode.
* `--mode`, `-m`: Analysis filter: `single-speaker`, `two-speakers`, `any-face`, `generic`, `others`.
* `-o`: Output folder name (default: `trimmed_clips`).
* `--custom-target-faces`: Custom face count for `others` mode (e.g. `0`, `3`, `>=1`).
* `--custom-min-frequency`: Custom face presence frequency (e.g. `0.8`).
* `--custom-min-size`: Custom minimum face width ratio (e.g. `0.08`).
