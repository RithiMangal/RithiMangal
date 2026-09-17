# ◉ LET AUDIO — `LA-9000 PRO`

### For all Cowon JetAudio lovers on Linux — the wait is over.

![Rust](https://img.shields.io/badge/built_with-RustCE422B?style=for-the-badge&logo=rust&logoColor=white)
![Linux](https://img.shields.io/badge/platform-Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![License](https://img.shields.io/badge/license-MIT-00e5ff?style=for-the-badge)

> You remember it. The spectrum analyzer dancing to your bassline. The 10-band
> equalizer sculpted genre by genre. The X-Bass button that made cheap
> headphones feel expensive, and the reverb that turned your bedroom into a
> concert hall. On Linux, nothing ever quite filled that hole — players were
> either minimal to a fault, or streaming wrappers wearing a trench coat.
>
> **Let Audio is the answer.** A from-scratch, Rust-powered hi-res music
> console for Linux, built around the classic JetAudio *workflow* you already
> love — transport, playlist, 10-band EQ, spectrum, and sound effects — wrapped
> in an original 3D sound-system faceplate with an amber LCD, real VU meters,
> and a rotary master knob. No Electron. No web views. One 17 MB native binary
> that opens instantly and stays out of your music's way.

*Let Audio is an independent, original implementation. It is not affiliated
with, endorsed by, or copying any assets from COWON or JetAudio — only the
love for the workflow is shared.*

---

## ✨ Why you'll feel at home

| You loved in JetAudio | You'll find in Let Audio |
|---|---|
| 10-band graphic EQ + genre presets | 10-band EQ (31 Hz – 16 kHz) + **32 presets**, applied instantly on select |
| X-Bass / X-Wide / Reverb | **Real DSP** — shelving biquads, mid/side widener, Schroeder reverb (Room / Hall / Stadium) |
| Dancing spectrum | Full-width **LED analyzer strip** + per-channel **VU meters with peak-hold** |
| Repeat one / shuffle / A-B | Repeat Off / All / **dedicated 🔂 repeat-track toggle** / Shuffle + A-B section repeat |
| Browser by artist / album / genre / folder | Library with Songs / Artists / Albums / Genres / Folders / ♥ Favorites / Recent + search |
| Sleep timer, speed control, lyrics, tag editor | All present — plus M3U playlists, drag & drop, and ffmpeg-powered conversion |

---

## 🎛️ The front panel

Boot it and you're staring at hardware, not a webpage:

- **Amber LCD display** — smoked glass, scanlines and all. Track number,
  title, artist – album – format, live status flags (`REPEAT·1`, `REV·HALL`,
  `BASS+40`, `BAL·L20`, `SLEEP·30`), and a glowing progress bar with time.
- **TRANSPORT deck** — ⏮ ▶/⏸ ⏹ ⏭ machined keys, repeat cycler, the 🔂
  repeat-current-track toggle, A-B section looper, and 50–200% speed control.
- **MASTER rotary knob** — grab it and *turn* it like real hardware (the
  pointer follows your cursor; no jumps, no dead-zone snaps). Hover + mouse
  wheel = fine trim. Below it: mute, volume readout, and a **real balance**
  control.
- **OUTPUT meters** — two live VU bars (L/R) with peak-hold needles and a
  CLIP LED, fed by actual per-channel peaks from the DSP chain — not an
  animation.
- **LED spectrum strip** — 28 columns × 8 rows of green-amber-red segments,
  the way analyzers are supposed to look.
- **Borderless chassis** — no OS window frame. Drag the titlebar to move,
  double-click to maximize, and power off with the red **⏻ OFF** key.
  Corner screws included, as God intended.

---

## 🔊 A real DSP engine, not checkboxes

Every sample of every track flows through `src/dsp.rs` — a hand-rolled,
zero-dependency DSP chain (shared live with the UI, so sliders bite
*mid-track* with no restart, no clicks):

- **X-Bass (0–100)** → RBJ-cookbook low-shelf biquad @ 120 Hz, 0…+12 dB
- **X-Treble (0–100)** → RBJ high-shelf biquad @ 9 kHz, 0…+12 dB
- **X-Wide** → mid/side stereo widener
- **Reverb (Room / Hall / Stadium + Wet)** → Schroeder/Moorer topology —
  8 damped feedback combs per channel into 4 series allpasses (Freeverb
  tunings, sample-rate scaled), with per-preset decay, damping and wet ceiling
- **Balance (−100…+100)** → true L/R output pan with smoothed gains
- **De-zippered everything** — parameters are slew-limited (~30 ms) so
  slider moves never click; filter/delay state resets cleanly on seek
- **AGC + preamp** ride on top for even loudness; clip guard keeps boosted
  bands out of digital overs

Formats: MP3 · WAV · OGG · FLAC · M4A · AAC · OPUS · APE · WV · TTA · MPC ·
AIFF (decode breadth follows the Symphonia/rodio backend).

---

## 📚 Library, playlists & the rest

- **Library browser** — Songs, Artists, Albums, Genres, Folders, ♥ Favorites,
  Recent, with instant search. Hearts toggle favorites inline.
- **Now-playing queue** — reorder (⬆/⬇), multi-select remove, shuffle list,
  clear, and **M3U save/load** for your lovingly curated collections.
- **Lyrics & tags** — embedded unsynchronised-lyrics viewer plus a tag editor
  (title / artist / album / genre) that writes back to the file.
- **Convert** — one click to MP3 / FLAC / OGG / WAV via `ffmpeg`
  (if installed).
- **The comforts** — sleep timer (15/30/60/120 min), gapless toggle,
  crossfade, fade in/out, playback-speed with pitch behavior, global Space =
  play/pause, double-click = play, drag & drop files *and* folders anywhere,
  CLI arguments (`let-audio ~/Music/loop.mp3`), and settings that persist to
  `~/.config/let-audio/config.json`.

<details>
<summary><b>🎚️ All 32 EQ presets</b></summary>

Flat · Pop · Rock · Jazz · Classic · Vocal · Bass Booster · Treble Booster ·
Dance · Club · Party · Live · Concert Hall · Headphones · Laptop Speakers ·
Large Speakers · Small Speakers · Acoustic · Blues · Country · Electronic ·
Hip-Hop · Latin · Metal · R&B · Reggae · Soul · Alternative · Punk · Funk ·
X-Bass Boost · Crystal Clear

(All curves are our own tuning — pick one and it applies instantly; touch a
slider and it becomes *Custom*.)
</details>

---

## 🚀 Get running in 60 seconds

**Option A — portable tarball (any distro, no root):**
```bash
tar -xzf let-audio-1.0.0-linux-x86_64.tar.gz
cd let-audio-1.0.0
./install.sh        # → ~/.local/bin/let-audio + app-menu entry + icon
let-audio           # make sure ~/.local/bin is on your PATH
```

**Option B — straight from source:**
```bash
cargo build --release
./target/release/let-audio ~/Music
```

**Daily workflow:**
1. `＋ Folder` → point at your music. The library builds itself.
2. Double-click anything — the LCD lights up, the needles jump.
3. Pick an EQ preset, twist X-Bass to taste, switch reverb to **Hall**,
   lean back.
4. ♥ the keepers, `💾 M3U` the weekend playlist, set `⏾ Sleep 30` for bed.

| Shortcut | Action |
|---|---|
| `Space` | Play / pause |
| Double-click a row | Play it now |
| Drag & drop | Add files / folders from anywhere |
| Wheel over the knob | Fine volume trim |

---

## 🖥️ Distro notes

One `x86_64` glibc binary runs on Ubuntu, Debian, Fedora, Arch, openSUSE,
Mint, Pop!_OS and friends. It links only against the usual suspects
(ALSA, X11/Wayland, OpenGL). If audio won't start, install your distro's
sound + X11 client libs (and `ffmpeg` if you want Convert):

| Distro | One-liner |
|---|---|
| Debian / Ubuntu / Mint / Pop!_OS | `sudo apt install libasound2 libx11-6 libxi6 libxrandr2 libxcursor1 libxinerama1 ffmpeg` |
| Fedora | `sudo dnf install alsa-lib libX11 libXi libXrandr libXcursor libXinerama ffmpeg` |
| Arch | `sudo pacman -S alsa-lib libx11 libxi libxrandr libxcursor libxinerama ffmpeg` |

Prefer native? `./packaging/build-portable.sh` rebuilds and re-tarballs on
your machine.

---

## 🧪 Under the hood

```
src/
  main.rs   # faceplate UI (egui), transport, library, playlist, EQ, persistence
  dsp.rs    # real-time DSP: shelves, widener, Schroeder reverb, pan, VU peaks
packaging/  # .desktop entry, original icon, install.sh, build-portable.sh
dist/       # portable tarball (generated)
```

Quality is enforced, not promised — `cargo test` covers the DSP core:
flat-chain transparency, bass/treble energy lift on test tones, reverb
audibly altering the signal, hard-pan muting the opposite channel, and VU
peak reporting. **6/6 green.**

---

## 🗺️ Roadmap

- Per-band EQ wired into the DSP chain (the 10 sliders currently drive
  loudness compensation + presets; shelves are next)
- True gapless/crossfade mixer, ReplayGain scan, album-art display
- Mini-player mode, MPRIS/D-Bus + media-key support, system tray
- Windows build (the code is portable; the CI isn't — yet)

---

## 📜 License

MIT. Crank it up. 🔉
