# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a single-file static web application for The Life Church Memphis Auditorium. It provides an interactive floor plan reference tool for broadcast/live event technicians to look up stage box connections and wiring throughout the auditorium.

## Development

No build system, package manager, or dependencies. Open `index.html` directly in a browser — that's it.

## Architecture

The entire application lives in `index.html` (~557 lines). There are no external scripts, frameworks, or stylesheets (only Google Fonts via CDN).

### Core Data Structures

- **`boxes`** — JavaScript object with 22 box entries, each containing `name`, `type`, `label`, and a `sections` array of connection groups (audio, video, data, fiber, RF, etc.)
- **`destinations`** — Object mapping `"boxId||portName"` keys to user-assigned destination strings, persisted in LocalStorage under key `tlc_dest`

### Box Types & Colors

| Type | CSS var | Examples |
|------|---------|---------|
| Wall Box (WB) | `--accent-amber` (#d4b74a) | Stage sides, walls, camera platform |
| Stage Floor Box (FBST) | `--accent-purple` | Microphone/guitar positions |
| Floor Box (FB) | `--accent-green` | Camera monitoring positions |
| FOH | `--accent-red` | Front-of-house booth positions |

### Key UI Components

1. **SVG Floor Plan** — 680×560 canvas with clickable box buttons; clicking selects a box and populates the detail panel
2. **Detail Panel** — Shows port/connection info for the selected box, with color-coded tags per connection type
3. **Accordion/List View** — All boxes expandable, shows assigned vs. total port counts; driven by the same `boxes` data
4. **Search** — Real-time filter across box IDs, names, and port names; highlights matches
5. **Modal Editor** — Edit destination text for any port; saves to LocalStorage and updates `tlc_updated` timestamp

### LocalStorage Keys

- `tlc_dest` — JSON object of user-edited destinations (`"boxId||portName"` → string)
- `tlc_updated` — ISO timestamp of last save
