<div align="center">

<img src="screenshots/logo.png" width="96" alt="Cessator" />

**[English](README.md)** · **[Русский](README_RU.md)**
# Cessator

**Honest time statistics for your PC.**

*Cessator (Latin) - "one who has ceased", an idler at rest*

How long each app stays open, in focus, and playing sound.
Plus live Steam friends monitoring and post-match Dota 2 analytics.
Even tracks activity per browser tab, not just whether the browser is open or closed.

PS The code stays private not because I'm greedy - I just don't want to accidentally leak personal info)

![Windows](https://img.shields.io/badge/Windows-10%2F11-0078D6?logo=windows11&logoColor=white)
![Local data](https://img.shields.io/badge/data-100%25%20on%20your%20PC-2ea44f)
![No cloud](https://img.shields.io/badge/no-cloud%20or%20accounts-e0a832)

</div>

---

![Cessator dashboard - day overview](screenshots/demo-overview.png)

<sub>Day overview: focus per app, category ring, and the browser split by site - no extension required.</sub>

## Features

### Time tracker

- **Focus and background** - count time for the active window only, or for
  everything that is open. Periods: today, 12h, 24h, 7 days, or a custom
  range with a calendar and activity highlighting.
- **Browser tabs - no extension.** The address bar of the focused window
  is read via Windows UI Automation: the browser expands into sites
  (youtube.com, github.com, ...). Works in Chrome, Edge, Brave, Vivaldi
  and Firefox.
- **Day timeline** - the whole day on one screen: app lanes, focus and
  sound layers, away periods, tracker start marks.
- **A game is running right now** - local detection of a running game
  with a live timer: the card appears ~5 seconds after launch.
- **"Away / back"** by input idle; sound while focused cancels away.

![Day timeline](screenshots/demo-timeline.png)

<sub>Day timeline: wheel to zoom, drag to pan, short focuses hidden by a threshold.</sub>

### Catalog

- **Custom names** for apps - "code.exe" becomes "Visual Studio Code".
- **Categories and hiding** for apps and individual sites: a rule, an
  override from the dashboard, or hide completely - the data stays.
- Browsers expand by tabs: a category and data removal for each site
  separately.

![App and site catalog](screenshots/demo-catalog.png)

<sub>Catalog: Google Chrome expanded by sites - each with its own category and visibility.</sub>

### Steam

- **Playing now, library, friends online and in-game** - all metrics on
  one screen.
- **Friend sessions per day** - a row per friend, lanes per game,
  "game · time · duration" tooltips, pick a day with the calendar.
- **Custom names for friends** and **hiding**: a renamed friend shows
  everywhere instead of the nickname; a hidden one is not polled and
  never gets in the way.
- A "track friends" switch - stops polling entirely.

### Dota 2

- **Match list** as a period summary: W/L, winrate bar, average KDA,
  most played hero, a 12-week heatmap.
- **Match overview** - your card, both teams' scoreboard with positions,
  KDA, damage and networth, items, draft, benchmarks against the rank.
- **Match timeline** - objectives, fights and purchases on one scale
  with zoom-by-selection and a synced playhead.
- **Sources**: OpenDota (no key) and the STRATZ PRO layer (a free
  personal token) - IMP, win probability by minute, gold sources,
  CS@10/20.

### Settings

- 4 interface styles (Classic / Playful / Terminal / Player) × dark and
  light themes, category colors, navigation and tab styles.
- **EN / RU** language, day boundary (night sessions go to yesterday).
- **Autostart with Windows** - a switch here and a checkbox in the tray
  menu, always in sync.
- **Friendly address** `cessator.local` with no port in the URL.
- **Phone access** over Wi-Fi - the dashboard opens from a phone on the
  same network.
- Update banner: the source is baked in (the project's GitHub) - when a
  new version is out, a download link appears; you can point it at your
  own repo or a `latest.json`. The current version is in the dashboard
  header.
- Full uninstall in two clicks, with an option to wipe the data.

![Cessator settings](screenshots/demo-settings.png)

<sub>Settings: autostart, friendly address, phone access and the day boundary.</sub>

## Privacy

- **No cloud, no accounts, no telemetry.** The dashboard is a local
  server on your PC.
- All statistics live in a local SQLite database in a folder you choose.
- Steam / STRATZ keys are stored only in the local config and never
  reach the browser.
- Network access is off by default and enabled explicitly.

## Install

1. Download `Cessator.exe` from [Releases](../../releases) - a portable
   file, no installer: put it anywhere and run it.
2. On first launch the app asks for a folder for the local database
   (default: `%LOCALAPPDATA%\Cessator`).
3. The dashboard opens at `http://127.0.0.1:8777`, the tray icon appears.
4. Want an address without a port? Enable "Friendly address
   (cessator.local)" in Settings: Windows asks for one admin
   confirmation.

Windows may warn about an unknown publisher (the exe is unsigned) -
"More info → Run anyway".

Updating - download the new `Cessator.exe` and replace the old file:
data, keys and settings live separately from the program and are never
touched. Previous ChronoTrack installs pick everything up themselves -
statistics, keys and autostart migrate automatically.

## How it works

Every ~5 seconds a snapshot of top-level visible windows is taken (like
Alt-Tab), plus a per-process sound flag and the global input idle. When
a window's state changes, the row in the database is closed and a new
one opens. Steam friends are polled every 90 seconds; Dota matches are
pulled from OpenDota/STRATZ after a game ends.

The app sits in the tray: open dashboard, pause tracking, autostart,
quit.

## System load

The tracker is designed to be invisible: one tray process, a window
snapshot every 5 seconds, and tiny writes to a local SQLite database.
The web dashboard works on demand - while the tab is closed, it does
nothing.

Measured on a regular home PC (12 threads, a live 5-day database):

![System load](screenshots/system-load.png)

<sub>Measurement: process metrics, CPU compared to a background Chrome tab, and database growth on disk.</sub>

| Metric | Value |
|---|---|
| CPU | **0.03%** of the system (0.4% of one core) |
| RAM | **~57 MB** for both processes |
| Disk (I/O) | **~0** at idle - writes only when a window changes |
| One poll cycle | **~18 ms** every 5 seconds |
| Network at idle | ~0 - port 8777 just listening |

For comparison: one background Chrome tab with active JS takes ~1.5% of
a core - several times more than all of Cessator.

Database growth - from live data (~6,700 rows per day; a row opens only
when a window's state changes, not on every poll):

| Period | Statistics DB |
|---|---|
| Week | ~6 MB |
| Month | ~27 MB |
| Year | ~330 MB |

Steam / Dota / STRATZ caches add a few megabytes.

---

<div align="center">

**Cessator** - time speaks for itself.

</div>
