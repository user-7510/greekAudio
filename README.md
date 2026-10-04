# greekAudio

[English](README.md) | [繁體中文](README.zh-TW.md)

A terminal UI (TUI) for Termux that looks up Biblical Greek words and plays their pronunciation. Search by Greek script, romanization, transliteration, or English gloss, pick a word by keyboard or tap, and the audio is downloaded with yt-dlp, cached locally, and played with `termux-media-player`.


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
- Keyboard and touch input: tap a row to play, scroll with the wheel
- Local cache: cached words are marked with `*` and play without downloading again
- Background playback through `termux-media-player`
- `Ctrl+D` clears the cache after a Y/n confirmation
- Single `.py` file: the word list is embedded in compressed form and extracted to `.greek.csv` on first launch
- Python standard library only


## Tech Stack

**Language:** Python 3.7+ (standard library only)  
**Downloader:** yt-dlp  
**Playback:** Termux:API (`termux-media-player`)


## Installation

Install the Termux packages:

```bash
pkg install python termux-api
```

Install the Termux:API Android app as well; `termux-media-player` does not work without it.

Install yt-dlp:

```bash
pip install -r requirements.txt
```


## Usage

Start the program:

```bash
python greekAudio.py
```

To use a different word list, pass a CSV path (see the Appendix for the required columns):

```bash
python greekAudio.py words.csv
```

### Controls

| Key or action | Effect |
| --- | --- |
| Type text | Filter results instantly |
| `Enter` or tap a row | Download if needed, then play |
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
| `.greek.csv` | Word list extracted on first launch (delete it to regenerate) |
| `.cache/` | Downloaded audio, named `romanization_videoId.ext` |


## FAQ

#### How do I stop playback?

Run `termux-media-player stop`. Playing another word also stops the previous one.

#### Download or playback fails

Update yt-dlp with `pip install -U yt-dlp`, and confirm that both the `termux-api` package and the Termux:API app are installed.

#### Why is the audio not converted?

Audio is played as downloaded (m4a is preferred), so ffmpeg is not required.


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

- Termux:API APK: [https://github.com/termux/termux-api/releases/download/v0.53.0/termux-api-app_v0.53.0+github.debug.apk](https://github.com/termux/termux-api/releases/download/v0.53.0/termux-api-app_v0.53.0+github.debug.apk)
