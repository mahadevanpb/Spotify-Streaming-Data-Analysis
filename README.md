# 🎵 Spotify Streaming Data Analysis — Project Overview

## 📁 Project Structure

| File | Size | Description |
|------|------|-------------|
| `Spotify Streaming.ipynb` | ~331 KB | Main Jupyter Notebook with all analysis code |
| `spotify_history.csv` | ~19.7 MB | Raw Spotify listening history dataset |

---

## 📊 Dataset Summary

### Source File: `spotify_history.csv`
- **Total Records:** 149,860 listening events
- **Time Range:** July 2013 – December 2024 (11+ years of data!)
- **Primary Platform:** Android (recent years), Web Player (early data)

### Dataset Schema (11 Columns)

| Column | Type | Description |
|--------|------|-------------|
| `spotify_track_uri` | object | Unique Spotify track identifier (dropped early in analysis) |
| `Time Stamp` | object → datetime | When the track was played |
| `platform` | object | Device/app used (`android`, `web player`) |
| `ms_played` | int64 | Milliseconds the track was played |
| `track_name` | object | Song title |
| `artist_name` | object | Artist name |
| `album_name` | object | Album name |
| `reason_start` | object | Why playback started (`trackdone`, `clickrow`, `fwdbtn`, `autoplay`, `remote`) |
| `reason_end` | object | Why playback ended (`trackdone`, `clickrow`, `fwdbtn`, `nextbtn`, `unknown`) |
| `shuffle` | bool | Whether shuffle was active |
| `skipped` | bool | Whether the track was skipped |

> **Derived Column:** `playtime_sec` = `ms_played / 1000`

---

## 🔬 Analysis Performed

### 1. Data Loading & Cleaning
- Loaded CSV with pandas
- Dropped `spotify_track_uri` (not needed for analysis)
- Converted `ms_played` → `playtime_sec` (seconds)
- Parsed `Time Stamp` to `datetime` format

### 2. Top 10 Artists by Total Playtime (All Time)

| Rank | Artist | Playtime (seconds) |
|------|--------|--------------------|
| 1 | The Beatles | 1,210,184 |
| 2 | The Killers | 1,059,556 |
| 3 | John Mayer | 725,219 |
| 4 | Bob Dylan | 569,456 |
| 5 | Paul McCartney | 357,354 |
| 6 | Howard Shore | 348,930 |
| 7 | The Strokes | 317,508 |
| 8 | The Rolling Stones | 307,917 |
| 9 | Pink Floyd | 260,531 |
| 10 | Led Zeppelin | 248,338 |

### 3. Top 10 Artists — 2023 vs 2024 Comparison
- **Subset used:** 20,893 rows (Jan 2023 – Dec 2024)
- **Visualization:** Grouped bar chart comparing playtime per artist across 2023 and 2024
- **Colors used:** `#E14434` (red) and `#77BEF0` (blue)

> ⚠️ Known bug in notebook: An `AttributeError: 'int' object has no attribute 'set_hatch'` occurs when applying hatch patterns to bars. The chart still renders but without hatch styling.

### 4. Top 10 Songs by Total Playtime + Skip Count

- Aggregated by `track_name` to compute:
  - `total_playtime_sec` — cumulative listening time
  - `total_skips` — how many times the track was skipped
- Results include information from the entire listening history (not filtered to 2023–2024)
- Scatter chart rendered with bubble markers showing both playtime and skip frequency

---

## 🛠️ Tech Stack

| Tool | Version |
|------|---------|
| Python | (standard env) |
| pandas | data wrangling |
| matplotlib | plotting |
| seaborn | styling |
| datetime | timestamp handling |

---

## 🐛 Known Issues

| Issue | Location | Description |
|-------|----------|-------------|
| `AttributeError` | Cell 17 | `bars.set_hatch(...)` fails because iterating over DataFrame columns yields column names (strings), not bar containers |
| `SettingWithCopyWarning` | Cell 12 | `df_filtered['year'] = ...` on a copy of a slice — should use `.loc[]` |

---

## 💡 Potential Next Steps

- [ ] Fix the `set_hatch` bug (iterate over `ax.containers` instead of `plot_data`)
- [ ] Fix the `SettingWithCopyWarning` by using `.loc[]` assignment
- [ ] Analyze listening trends by **time of day** or **day of week**
- [ ] Compare **shuffle vs. non-shuffle** skip rates
- [ ] Build a **monthly listening heatmap** for the full 11-year period
- [ ] Explore **album-level** statistics
- [ ] Identify **most-skipped artists** vs. most-played artists
- [ ] Calculate **completion rate** per song (ms_played / track_duration)
