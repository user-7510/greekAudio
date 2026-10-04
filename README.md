# greekAudio

[English](README.md) | [繁體中文](README-ZH.md)

A terminal UI (TUI) that looks up Biblical Greek words and plays their pronunciation. Search by Greek script, romanization, transliteration, or English gloss, pick a word by keyboard or mouse/tap, and the audio is downloaded with yt-dlp, cached locally, and played. It runs on Termux (Android), Ubuntu desktop, macOS, and Windows, with one dedicated script per platform.


## Features

- Searches 4,599 entries (4,598 Greek words plus the Greek alphabet song)
- Matches a query against all of the following at once:
  - Romanization (case-insensitive, ignores macrons)
  - Greek script (ignores accents, breathing marks, and final sigma)
  - Plain letter-by-letter transliteration of the Greek (for example `χ` to `ch`, `φ` to `ph`)
  - Greek-keyboard-layout transliteration (for example `χ` to `x`, `η` to `h`, `ς` to `w`)
  - English gloss
- Multi-word queries require every word to match; results are ranked by exact, prefix, word-prefix, then substring match
- Fuzzy fallback for typos when there are fewer than 10 matches and the query has at least 3 characters
- No cap on the number of results
- Keyboard and mouse/touch input: click or tap a row to play, scroll with the wheel
- Local cache: cached words are marked with `*` and play without downloading again
- `Ctrl+D` clears the cache after a Y/n confirmation
- Single `.py` file per platform: the word list is embedded in compressed form and extracted to `.greek.csv` on first launch
- Python standard library only (plus `windows-curses` on Windows)


## Platforms

| Script | Platform | Playback |
| --- | --- | --- |
| `greekAudio.py` | Termux (Android) | `termux-media-player`, in the background |
| `greekAudio_linux.py` | Ubuntu desktop | `xdg-open` (system default player) |
| `greekAudio_mac.py` | macOS | `open` (system default player) |
| `greekAudio_windows.py` | Windows | `os.startfile` (system default player) |

All scripts share the same search, interface, and cache behavior; only playback differs.


## Tech Stack

**Language:** Python 3.7+ (standard library only; Windows also needs `windows-curses`)  
**Downloader:** yt-dlp  
**Playback:** Termux:API on Termux, the system default player on desktop


## Installation

`requirements.txt` installs yt-dlp, and installs `windows-curses` only on Windows. yt-dlp is run as a Python module (`python -m yt_dlp`) when it is installed for the same Python, otherwise as a `yt-dlp` command found on `PATH`.

### Termux

```bash
pkg install python termux-api
pip install -r requirements.txt
```

Install the Termux:API Android app as well (see Downloads); `termux-media-player` does not work without it.

### Ubuntu desktop

```bash
sudo apt install python3 python3-venv xdg-utils
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

A virtual environment is used because recent Ubuntu releases may refuse system-wide `pip install`.

### macOS

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### Windows

```powershell
py -m pip install -r requirements.txt
```


## Usage

Start the script for your platform:

```bash
python greekAudio.py
python3 greekAudio_linux.py
python3 greekAudio_mac.py
py greekAudio_windows.py
```

To use a different word list, pass a CSV path after the script name (see the Appendix for the required columns).

### Controls

| Key or action | Effect |
| --- | --- |
| Type text | Filter results instantly |
| `Enter` or click/tap a row | Download if needed, then play |
| `Up`, `Down`, `Ctrl+P`, `Ctrl+N` | Move the selection |
| `PgUp`, `PgDn`, `Home`, `End` | Jump through the list |
| `Ctrl+U` | Clear the input |
| `Ctrl+D` | Clear the cache (asks Y/n) |
| `Esc`, `Ctrl+C` | Quit |

### Search examples

| Input | Matched through |
| --- | --- |
| `biblos` | Romanization |
| `βιβλ` | Greek script |
| `xaris` | Greek-keyboard transliteration of `χάρις` |
| `grace` | English gloss |

### Files created

| Path | Description |
| --- | --- |
| `.greek.csv` | Word list extracted on first launch, next to the script (delete it to regenerate) |
| `.cache/` | Downloaded audio, named `romanization_videoId.ext`, next to the script |


## FAQ

#### How do I stop playback?

On Termux, run `termux-media-player stop`; playing another word also stops the previous one. On desktop the audio opens in your default player, so close or stop it there; the program cannot stop it.

#### Download or playback fails

Update yt-dlp with `pip install -U yt-dlp`. On Termux, also confirm that both the `termux-api` package and the Termux:API app are installed. On Ubuntu desktop, confirm that `xdg-open` exists.

#### Why is the audio not converted?

Audio is played as downloaded (m4a is preferred), so ffmpeg is not required. If your default player on desktop cannot play a file or shows a duration of 00:00, installing ffmpeg may help; this has not been tested.


## Acknowledgements

 - [yt-dlp](https://github.com/yt-dlp/yt-dlp)
 - Pronunciation videos by Logos Bible Software on YouTube. No audio is bundled with this project; it is downloaded on demand.


## Appendix

The word list is a UTF-8 CSV with a header row and these columns:

| Column | Description |
| --- | --- |
| `url` | YouTube watch URL of the pronunciation video |
| `romanization` | Romanized form of the word |
| `greek` | Greek spelling (empty for the alphabet song) |
| `gloss` | English meaning |


## Downloads

- Termux:API APK (Termux only): [https://github.com/termux/termux-api/releases/download/v0.53.0/termux-api-app_v0.53.0+github.debug.apk](https://github.com/termux/termux-api/releases/download/v0.53.0/termux-api-app_v0.53.0+github.debug.apk)
