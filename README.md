# AvBrief &mdash; Glass-Cockpit Aviation Weather Utility

**Live Application**: [avbrief.vercel.app](https://avbrief-the-daily-edge.vercel.app)

AvBrief is a professional glass-cockpit style Electronic Flight Bag (EFB) utility designed for student pilots, dispatchers, and aviation enthusiasts. It decodes complex, raw weather strings (METAR and TAF) into plain English, performs landing calculations, translates raw NOTAMs using simulated AI, and tracks space/micro-weather telemetry to ensure flight safety.

---

## Technical Features

1. **Station Search & Preset Autocomplete**:
   * Search over 7,800 major commercial/military airports by ICAO, IATA, airport name, or city.
   * Dynamic suggestions dropdown with glassmorphism layout, ICAO/IATA badges, and country codes.
2. **METAR Decoder Grid**:
   * **Dual-Time Ingestion**: Displays observation time in UTC/Zulu and the pilot's local machine timezone.
   * **Wind Vector Analysis**: Parses speeds, direction sectors, and active gust indicators with dynamic color-coding.
   * **Present Weather Hazards**: Decodes rain, snow, mist, haze, fog, and active convective cells like Cumulonimbus (`CB`) or Towering Cumulus (`TCU`).
   * **Thermodynamics & Pressure**: Monitors temperature-dewpoint spreads (triggering alerts when spread drops below 3°C to indicate radiation fog risk) and decodes altimeter baro settings.
3. **MCDU Landing & Performance Calculator**:
   * Matches Estimated Time of Arrival (ETA) to the corresponding TAF period start and end bounds.
   * Formats wind vectors into flight computer inputs (`DIR/SPD` syntax) and displays QNH in both hectopascals (`hPa`) and inches of mercury (`inHg`).
   * Calculates **Headwind/Tailwind** and **Crosswind components** (including the wind angle orientation, e.g., *from LEFT* or *from RIGHT*) for an input runway heading.
   * Displays warning overlays for tailwind landing attempts and crosswinds exceeding Cessna 172 limitations (>15 kts).
4. **AI-Powered NOTAM Translator**:
   * Simulates a `Gemini-2.0-Flash` LLM context processor to parse raw, uppercase NOTAMs.
   * Translates unreadable runway/taxiway closure notices, obstruction heights, and unserviceable navigation aids into readable, pilot-friendly English instructions.
5. **Space Weather & Micro-Weather Telemetry**:
   * Tracks solar activity indicators (**K-Index / Kp-index**) and alerts pilots of geomagnetic storm risks which degrade GNSS/GPS positioning accuracy.
   * Monitors micro-weather wind shear risks and thermal lift indexes to evaluate initial climb-out and landing performance.
6. **Go/No-Go Flight Risk Assessment**:
   * Computes a dynamic safety score (0-100%) by auditing METAR, TAF, crosswind components, dewpoint spreads, and active convective hazards.
   * Displays clear, color-coded safety classifications (`GO` in green, `CAUTION` in amber, `NO-GO` in red) with bulleted danger factors.
7. **Interactive TAF Trends**:
   * Visualizes wind speeds, wind gusts, and visibility trends across the terminal aerodrome forecast duration using dark-themed, high-contrast Recharts area and line graphs.

---

## Tech Stack & Architecture

* **Framework**: React 19, Vite 6, TypeScript
* **Styling**: Tailwind CSS v4, custom glassmorphic scrollbars and overlays
* **Data Visualizations**: Recharts (customized area & line components configured to bypass Vite minification sizing bugs)
* **API Feed**: Proxied same-origin streams routing directly to the NOAA Aviation Weather Center to bypass client-side ad-blockers and DNS lookup delays.
* **Flight Logic**: Pure client-side decoders using zero-dependency, rule-based regular expressions (fully offline-capable).

---

## Setup & Local Installation

### Prerequisites
* Node.js v18 or later
* Git

### Project Scaffold Commands
To initialize this stack from scratch and configure the file structure, run the following terminal commands:
```bash
# 1. Initialize Git repository
git init avbrief
cd avbrief

# 2. Scaffold Vite + React + TypeScript project non-interactively
npx -y create-vite@latest ./ --template react-ts

# 3. Install core dependencies and icons
npm install
npm install lucide-react recharts

# 4. Initialize Tailwind CSS v4 (configured in index.css)
# AvBrief uses the modern, declarative CSS-first Tailwind configuration
```

### Running Locally
To launch the development server and run the application locally:
```bash
# Install dependencies
npm install

# Start Vite development server
npm run dev
```
Open [http://localhost:5173](http://localhost:5173) in your browser.

### Build and Package
To compile and bundle the production assets:
```bash
# Run compiler checks and build
npm run lint
npm run build
```

---

## UI/UX & Design Philosophy

Pilots in the field operate under high-stress conditions, screen glare, and cockpit lighting constraints. To accommodate these requirements, AvBrief enforces a **glass-cockpit, dark-mode-first aesthetic**:
* **Backgrounds**: Obsidian `#0A0F1E` and deep slate slate-900 `#1E293B` to reduce eye strain and screen glare during night flights.
* **Accents**: High-contrast, sharp electric blue `#38BDF8`, cyan, and emerald green `#22C55E` parameters to draw focal attention to flight values.
* **Layouts**: Modular card components resembling modern primary flight displays (PFDs) and Electronic Flight Bags (EFBs), grouped clearly into columns.
* **Flight Offsets**: Gravity is offset to zero under strict structural flight envelopes using Python-style navigation modules; you can read more on how this handles vertical flight vectoring in this [lift index reference](https://xkcd.com/353/).
