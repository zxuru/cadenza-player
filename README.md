<div align="center">

<img src="packaging/cadenza.svg" width="110" alt="Cadenza">

<h1>Cadenza</h1>

<p><b>A local-files music player.</b><br>
Point it at a folder: it indexes what is there and plays it. No account, no
library in the cloud, and no network until you ask for one.</p>

<p>
  <a href="https://github.com/zxuru/cadenza-player/releases/latest"><img
     src="https://img.shields.io/github/v/release/zxuru/cadenza-player?color=7c5cff&label=release"
     alt="Latest release"></a>
  <a href="#build"><img
     src="https://img.shields.io/badge/platform-linux%20%7C%20windows%20%7C%20macos-7c5cff"
     alt="Platforms"></a>
  <a href="LICENSE"><img
     src="https://img.shields.io/github/license/zxuru/cadenza-player?color=7c5cff"
     alt="Licence"></a>
</p>

</div>

## Features

**Library**
- Recursive folder indexing into SQLite, tags read with TagLib.
- Rescans are incremental: a file is re-probed only when its `(mtime, size)`
  changed, so re-indexing an unchanged library is instant.
- Browse songs, artists, albums or genres. Search ignores case **and accents**,
  and a group row shows cover art, track and album counts and total time.

**Playlists**
- Every subfolder of the music folder is a playlist; picking one scopes the
  library and the search to that subtree.
- Rename in place: the folder is renamed with it, and the index, the selection,
  where downloads land and the queue keep up -- without stopping the music.
- Open the music folder in the desktop's own file manager, or walk it again
  (incrementally) from the rail.

**Playback**
- libmpv: gapless albums, and ReplayGain off, per track or per album.
- Bit-perfect output through `EXCLUSIVE`: WASAPI, CoreAudio, PipeWire.
- Shuffle over the list that is playing or over the whole library. The track on
  the playhead is never interrupted, and the mode is remembered.

**Lyrics**
- A `.lrc`/`.txt` file beside the track, then its own tags, then -- only if you
  switch lookups on -- [LRCLIB](https://lrclib.net); answers are cached in the
  index, so a track is asked about once.
- Timed lyrics follow the playhead, and clicking a line seeks to it.

**Get music**
- Search [YouTube Music](https://music.youtube.com), or paste a link to a
  release, a playlist or a recording.
- Results are resolved before they are listed -- cover, artist, year, releases
  first -- and what you pick is tagged (title, artist, album, cover) into the
  folder the sidebar has selected. A release arrives whole, in a folder of its
  own, which is a playlist here.
- `yt-dlp`, QuickJS and ffmpeg travel inside the packages; nothing else to
  install.

**Interface**
- Frameless, rounded and translucent, lit by the cover of the track that is
  playing. It draws its own title bar and window controls on Linux, Windows and
  macOS.
- Three views -- library, get music, settings -- one accent colour, and every
  radius, spacing step and type size in `src/ui/Theme.qml`.
- Every string comes from `locales/<code>.json`: drop a file in and it appears
  in the language list, with English as the fallback.

## Download

Every [release](https://github.com/zxuru/cadenza-player/releases/latest) carries
one file per platform, plus `SHA256SUMS`:

| File | Platform | How |
|---|---|---|
| `cadenza-<version>-x86_64.AppImage` | Linux | `chmod +x` and run. Bundles Qt, libmpv, TagLib and the tools; wants glibc 2.39+ (Ubuntu 24.04 or newer) |
| `cadenza-<version>-setup.exe` | Windows | everything inside, double-click to install |
| `cadenza-<version>-win64-portable.zip` | Windows | unzip and run |
| macOS | | built and notarised by hand: [packaging/macos](packaging/macos/README.md) |

`.deb` and `.rpm` are wired through CPack: `cd build && cpack -G DEB`.

## Build

Qt 6.5+, libmpv 0.37+, TagLib 1.13+, a C++20 compiler and CMake 3.21+.

```bash
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build --parallel
./build/cadenza
```

<details>
<summary><b>Packages per distribution</b></summary>

**Debian / Ubuntu**

```bash
sudo apt install build-essential cmake pkg-config \
    qt6-base-dev qt6-declarative-dev qt6-declarative-dev-tools qt6-svg-dev \
    qml6-module-qtquick qml6-module-qtquick-controls \
    qml6-module-qtquick-layouts qml6-module-qtquick-templates \
    qml6-module-qtquick-window qml6-module-qtquick-effects \
    qml6-module-qtquick-dialogs qml6-module-qtqml-workerscript \
    libmpv-dev libtag1-dev libsqlite3-dev
```

**Fedora**

```bash
sudo dnf install gcc-c++ cmake pkgconf-pkg-config \
    qt6-qtbase-devel qt6-qtdeclarative-devel qt6-qtsvg-devel \
    mpv-libs-devel taglib-devel sqlite-devel
```

**Arch**

```bash
sudo pacman -S base-devel cmake pkgconf qt6-base qt6-declarative qt6-svg \
    mpv taglib sqlite
```

**macOS**

```bash
brew install qt mpv taglib pkg-config cmake ninja
```

**Windows**

See [packaging/windows](packaging/windows/README.md).

</details>

### Command line

```
cadenza [folder]        index this folder instead of ~/Music (or ~/Música)
-d, --database <path>   use another index file than the default
--scan                  index the folder and exit, without opening a window
--check-i18n [lang...]  check the locale files against English
--version
```

## Licence

GPL-3.0-or-later -- see [LICENSE](LICENSE). libmpv is GPLv2+ as distributed by
Linux distributions and the official Windows builds; Qt 6 is LGPLv3; TagLib is
LGPLv2.1/MPL. All are compatible with a GPL-3.0 application.
