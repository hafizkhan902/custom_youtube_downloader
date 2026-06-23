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
   * **macOS:** Install via Homebrew:
     ```bash
     brew install ffmpeg
     ```
   * **Linux (Ubuntu/Debian):**
     ```bash
     sudo apt update && sudo apt install ffmpeg
     ```
   * **Windows:**
     Open PowerShell as an Administrator and run the following commands to automatically download, extract, and register FFmpeg in your PATH environment variable:
     ```powershell
     # Download the latest FFmpeg release essentials build
     curl -L -o ffmpeg.zip https://www.gyan.dev/ffmpeg/builds/ffmpeg-release-essentials.zip

     # Extract it to your C Drive
     Expand-Archive ffmpeg.zip -DestinationPath C:\

     # Rename the extracted folder for simpler PATH referencing
     Get-ChildItem -Path C:\ -Filter "ffmpeg-*-essentials_build" | Rename-Item -NewName "ffmpeg"

     # Add FFmpeg bin directory to your User PATH
     [System.Environment]::SetEnvironmentVariable("PATH", [System.Environment]::GetEnvironmentVariable("PATH", "User") + ";C:\ffmpeg\bin", "User")
     ```
     *(Note: Restart your PowerShell or Command Prompt terminal after running these commands to load the new PATH variable).*

---

## Installation

1. Clone the repository:
   ```bash
   git clone <your-github-repo-url>
   cd <your-repo-folder-name>
   ```

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
