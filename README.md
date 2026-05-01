# InsureMatch 🚗

**Auto Insurance Comparison Tool — Canada & USA**

A fully self-contained single-file web application for comparing auto insurance rates across all Canadian provinces/territories and all 50 US states, with live vehicle data powered by the NHTSA API and AI-driven valuations via Claude.

🔗 **Live site:** [suhelm.github.io/insurematch](https://suhelm.github.io/insurematch)

---

## Features

### 🏷️ Insurance Rate Comparison
- 6 Canadian insurers (Sonnet, Belairdirect, TD, CAA, Intact, Aviva)
- 6 US insurers (GEICO, State Farm, Progressive, Allstate, USAA, Liberty Mutual)
- Sorted lowest price first with pros, cons, and star ratings
- Rates adjust dynamically based on country, province/state, and coverage level
- Ontario postal code risk zones (Brampton vs Kitchener-Waterloo vs rural)

### 🌍 Full Geographic Coverage
- **Canada:** All 10 provinces + 3 territories with individual rate adjustments
- **USA:** All 50 states + Washington D.C. with individual rate adjustments
- Switching country refreshes insurers, agents, currency, and postal/ZIP format
- Info banner shows insurance system type (private / public / hybrid), minimum liability, and rate vs national average

### 🔍 Know Your Vehicle — Live NHTSA API
Calls the official US government VIN database in real time:
- Full VIN decode: make, model, year, trim, body class, plant country
- Engine specs: displacement, cylinders, fuel type, drive type, transmission
- Safety equipment: ABS, ESC, traction control, airbag locations, blind spot monitoring
- **Live NHTSA safety star ratings** for front crash, side crash, rollover, side pole
- Insurance risk classification based on vehicle class

### ⚠️ Recalls — Live NHTSA API
Calls `api.nhtsa.gov/recalls` in real time using your actual VIN:
- Returns all open recall campaigns for your make/model/year
- Shows component affected, consequence, and remedy
- Green confirmation badge if no open recalls exist
- Works for any vehicle sold in North America

### 💰 Resale Value — Live AI Valuation
Calls the Anthropic Claude API to generate a real-time market valuation:
- Original MSRP, current private-sale range, dealer trade-in range
- Full depreciation schedule from model year to 2026
- Six resale factors specific to vehicle class and selected region
- CAD or USD pricing based on selected country
- Listing links: AutoTrader, Kijiji, CarGurus (Canada) or AutoTrader.com, CarMax, Cars.com (USA)

### 🔧 Find Mechanic — AI-Powered Search
Calls the Anthropic Claude API to find repair specialists:
- Detects vehicle class from name (European luxury, Japanese, American, EV, truck)
- Returns 6 mechanics near the entered ZIP/postal code
- Shows name, address, phone (tap-to-call), rating, distance, hours, price range
- Specialist tags match vehicle type (e.g. Mercedes-Benz certified, AMG engines)
- US and Canadian mechanics with appropriate certifications (ASE, OEM)

### 👥 Browse Agents
- 12 Canadian agents (2 per insurer) in Ontario cities
- 12 US agents (2 per insurer) across major US cities
- Live search by name, city, specialty, or insurer
- Registry links: FSRAO / RIBO (Canada) or NAIC (USA)

---

## How It Works

```
┌─────────────────────────────────────────────────────────┐
│                    Browser (index.html)                  │
│                                                          │
│  Insurance rates ──── Hardcoded JS data + math          │
│  Postal/ZIP zones ─── Hardcoded lookup table            │
│  Province/State ────── Hardcoded 63 region dataset      │
│  Browse Agents ─────── Hardcoded sample profiles        │
│                                                          │
│  Know Your Vehicle ──► NHTSA VIN API (free, public)     │
│  Recalls ───────────► NHTSA Recalls API (free, public)  │
│  Safety Ratings ────► NHTSA Ratings API (free, public)  │
│  Resale Value ──────► Anthropic Claude API              │
│  Find Mechanic ─────► Anthropic Claude API              │
└─────────────────────────────────────────────────────────┘
```

| Feature | Data source | Internet? |
|---|---|---|
| Insurance rate cards | Hardcoded JS + zone math | ❌ No |
| Postal / ZIP risk zones | Hardcoded lookup table | ❌ No |
| Province / State dropdown | Hardcoded 63-region dataset | ❌ No |
| Browse Agents | Hardcoded sample profiles | ❌ No |
| Know Your Vehicle | NHTSA `vpic.nhtsa.dot.gov` | ✅ Yes (free) |
| Recall data | NHTSA `api.nhtsa.gov/recalls` | ✅ Yes (free) |
| Safety star ratings | NHTSA `api.nhtsa.gov/SafetyRatings` | ✅ Yes (free) |
| Resale valuation | Anthropic Claude API | ✅ Yes (paid) |
| Find Mechanic | Anthropic Claude API | ✅ Yes (paid) |

---

## APIs Used

### NHTSA (National Highway Traffic Safety Administration)
Free, public, no API key required.

| Endpoint | Purpose |
|---|---|
| `vpic.nhtsa.dot.gov/api/vehicles/decodevinvalues/{VIN}?format=json` | VIN decode |
| `api.nhtsa.gov/recalls/recallsByVehicle?make=&model=&modelYear=` | Open recalls |
| `api.nhtsa.gov/SafetyRatings/modelyear/{year}/make/{make}/model/{model}` | Safety ratings |

### Anthropic Claude API
Used for AI-generated resale valuations and mechanic discovery.
- Model: `claude-sonnet-4-20250514`
- Endpoint: `api.anthropic.com/v1/messages`
- Requires: API key (handled by claude.ai when running inside the platform)

---

## Tech Stack

- **Pure HTML + CSS + JavaScript** — zero frameworks, zero dependencies
- **Single file** — `index.html` (1,985 lines, 32 JS functions)
- **Fonts** — Google Fonts: Playfair Display, DM Sans, DM Mono
- **Deployment** — GitHub Pages (static hosting, free)
- **No build step** — open the file directly in any browser

---

## Local Development

```bash
# Clone the repo
git clone https://github.com/suhelm/insurematch.git
cd insurematch

# Open directly in browser — no server needed
open index.html         # macOS
start index.html        # Windows
xdg-open index.html     # Linux
```

---

## Deploying Updates

```bash
# After making changes to index.html
git add index.html
git commit -m "describe what changed"
git push origin main
# GitHub Pages auto-deploys within 60 seconds
```

---

## Rate Adjustments by Region

**Canada — vs Ontario baseline:**
| Region | Adjustment |
|---|---|
| Brampton, ON | +35% |
| Toronto, ON | +20% |
| Mississauga, ON | +22–25% |
| Kitchener-Waterloo, ON | Baseline |
| Rural Ontario | −8–15% |
| Quebec | −30% |
| PEI | −25% |
| Northern territories | −26–30% |

**USA — vs national average:**
| State | Adjustment |
|---|---|
| Michigan | +45% |
| Louisiana | +40% |
| Florida | +30% |
| New York | +28% |
| Maine | −30% |
| Vermont | −28% |
| Ohio | −20% |
| Idaho | −28% |

---

## Disclaimer

Insurance rates shown are **estimates** based on published averages and are intended for comparison purposes only. Actual premiums depend on your driving history, age, claims record, and individual insurer underwriting. Always obtain quotes from a licensed broker before purchasing.

Resale valuations and mechanic listings are **AI-generated** and should be verified independently before any transaction.

---

## Author

**Suhel M.** — Senior Principal Consultant & Solutions Architect  
Kitchener-Waterloo, Ontario, Canada  
[github.com/suhelm](https://github.com/suhelm)

---

*Built with Claude AI · Powered by NHTSA public APIs · Deployed on GitHub Pages*
