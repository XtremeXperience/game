# Xtreme Xperience Supercar Racing Game Design

## 1) Vision
Build an official **Xtreme Xperience** web racing game where players:
- Race the exact supercars shown on your site.
- Enter race weekends mapped to your real event schedule and tracks.
- Progress from first-time driver to touring-series champion.
- Seamlessly click through to book real-world experiences.

Working title: **Xtreme Tour: Supercar Circuit**

---

## 2) Core Product Pillars
1. **Authentic Fleet**: Every driveable car mirrors your current supercar lineup.
2. **Real Event Calendar**: In-game race weekends track your live event pages/locations.
3. **Web-first Accessibility**: Fast-loading browser game, mobile + desktop.
4. **Experience Bridge**: Game progression nudges players toward booking real drives.

---

## 3) Target Platforms & Tech
- **Client**: Web app (desktop/mobile browser), built with React + TypeScript.
- **Game Engine Layer**: Phaser (arcade-sim handling) or Three.js + cannon-es (more realism).
- **Backend**: Node/TypeScript service for event sync, profiles, leaderboards.
- **Data**: Postgres for player + seasonal data; Redis for hot leaderboard caches.

Recommended launch architecture: **React + Phaser + Node API** for quick iteration and strong web performance.

---

## 4) Game Modes

### A. Tour Mode (Primary)
- Weekly/seasonal calendar generated from your real event list.
- Each event = venue + date window + race format.
- Players choose a car and run:
  - Qualifying lap
  - Sprint race
  - Feature race

### B. Time Attack
- Solo leaderboard by car + track + weather preset.
- Global and event-local ranking slices (e.g., “Dallas Weekend Ranking”).

### C. Head-to-Head Ghost Battles
- Async races against ghosts from top players.
- Lightweight multiplayer without live netcode complexity.

### D. Brand Challenge Series
- Special cups: Ferrari Cup, Porsche GT Challenge, Italian Legends, etc.
- Uses package themes already familiar to your customers.

---

## 5) Car Roster (Matched to Current Fleet)
Implement these as launch cars, with tuning classes derived from your public specs:

1. **Nissan GT-R**
2. **Corvette C8 Z06**
3. **Porsche Cayman GT4 RS**
4. **Lamborghini Huracan LP610-4**
5. **Ferrari 488 GTB**
6. **Porsche 911 GT3**
7. **Ferrari 296 GTB**

Optional non-race vehicle mode:
- **Charger Hellcat (Ride-Along)** as instructor/demo sequence car only.

### Car Handling Identity (Design Intention)
- GT-R: all-weather grip, forgiving exits.
- C8 Z06: high power + brake confidence.
- Cayman GT4 RS: precision cornering and high consistency.
- Huracan LP610-4: acceleration + traction confidence.
- 488 GTB: high-risk/high-reward pace car.
- 911 GT3: best all-around advanced driver car.
- 296 GTB: top-tier hybrid pace, highest mastery ceiling.

---

## 6) Event + Location Matching Strategy (Critical)

## Goal
Ensure the game always reflects your latest schedule and venue list from:
- `https://www.thextremexperience.com/events`

### Data Sync Design
1. **Event Ingest Worker (every 6 hours)**
   - Pulls your events source data (official API/export if available).
   - Fallback parser can read event URLs/slugs when API is unavailable.
2. **Normalizer**
   - Converts events into canonical fields:
     - `event_id`
     - `event_name`
     - `track_name`
     - `city`
     - `state`
     - `country`
     - `start_date`, `end_date`
     - `timezone`
     - `booking_url`
3. **Deduper + Versioning**
   - Upsert by external event id or stable slug hash.
   - Keep revisions so date/location changes can be audited.
4. **Game Calendar Publisher**
   - Publishes `current_season_events` for gameplay and UI.

### Seasonal Mapping Logic
- If an event date is within next 12 months: include in **Live Tour**.
- If event is in past: move to **Legacy Weekends** (practice + replay).
- If event is canceled/hidden: remove from ranked queues, keep private practice.

### Runtime UX
- Home screen card: “Next Real Event Near You”.
- Auto geolocation (opt-in) to prioritize nearby venues.
- Every event tile includes **Book Real Drive** CTA to matching track page.

---

## 7) Track Design Approach
- Create one “gameplay-optimized” layout per real venue (not laser scan at launch).
- Preserve signature corners and elevation cues for recognition.
- Track classes:
  - Beginner-friendly flow tracks
  - Technical precision tracks
  - High-speed power tracks

### Difficulty by Venue
- Bronze/Silver/Gold target times per car class.
- Dynamic assists (ABS, traction, racing line) tied to player license level.

---

## 8) Progression & Economy

### Driver License Ladder
- **Rookie → Club → Pro → Elite**
- Unlocks tougher events, reduced assists, and higher payout multipliers.

### Currency
- **XP**: skill progression.
- **Credits**: unlock liveries, performance packages, entry fees for elite events.

### Reward Types
- Car mastery badges by model.
- Track mastery badges by venue.
- Seasonal title banners (e.g., “Atlanta Sprint Champion”).

---

## 9) Booking Funnel Integration
This is the strategic differentiator.

- Event results screen includes:
  - “You just raced [Track]. Drive it in real life.”
  - Deep link to exact event location page.
- Car mastery page includes:
  - “Loved the 911 GT3? Book this exact car.”
- Offer code mechanics:
  - Milestone rewards can unlock real-site promo codes.

KPI targets:
- Game → booking click-through rate
- Booking conversion rate from game traffic
- Repeat session rate before purchase

---

## 10) Live Ops Plan

### Weekly
- Featured real-world venue challenge.
- Rotating one-make cup (single car challenge).

### Monthly
- “Tour Pack” update from latest event schedule sync.
- New cosmetic rewards tied to top 10% leaderboard percentile.

### Seasonal
- Championship reset with historical hall of fame snapshots.

---

## 11) Anti-Cheat + Fairness
- Server-authoritative lap validation (speed + checkpoint sanity).
- Ghost hash validation and anomaly detection.
- Segregate assisted vs no-assist leaderboard pools.

---

## 12) Accessibility + UX
- One-thumb mobile steering mode.
- Color-blind safe racing line and UI palette.
- Low-bandwidth mode with reduced effects.
- Session length target: 3–8 minutes per race loop.

---

## 13) MVP Scope (10–12 Weeks)

### Must Have
- 7-car matched roster.
- 8–12 launch tracks mapped from current event locations.
- Live event calendar ingestion pipeline.
- Tour Mode + Time Attack.
- Profile + cloud save + leaderboard.
- Booking CTA integration.

### Nice to Have
- Ghost battles.
- Clan/team system.
- Full telemetry overlays.

---

## 14) Content/Data Contracts

### Car Config Schema
```json
{
  "car_id": "ferrari_488_gtb",
  "display_name": "Ferrari 488 GTB",
  "class": "S",
  "top_speed_mph": 205,
  "zero_to_sixty_sec": 3.0,
  "horsepower": 661,
  "drivetrain": "RWD",
  "is_active": true
}
```

### Event Config Schema
```json
{
  "event_id": "2026-mar-atlanta-atlanta-motorsports-park-1",
  "track_name": "Atlanta Motorsports Park",
  "city": "Dawsonville",
  "state": "GA",
  "start_date": "2026-03-14",
  "end_date": "2026-03-15",
  "booking_url": "https://www.thextremexperience.com/events/2026-mar-atlanta-atlanta-motorsports-park-1/",
  "status": "scheduled"
}
```

---

## 15) Implementation Roadmap

### Phase 1 — Foundations (Weeks 1–3)
- Core physics, controls, camera, HUD.
- Car config system and first 3 cars.
- Track authoring pipeline.

### Phase 2 — Live Calendar + Tour (Weeks 4–7)
- Event ingestion and normalization service.
- Tour mode calendar UI + race flow.
- Ranking + replay ghost export/import.

### Phase 3 — Conversion + Polish (Weeks 8–10)
- Booking CTA instrumentation.
- Economy, unlocks, rewards.
- Performance optimization and anti-cheat checks.

### Phase 4 — Launch Hardening (Weeks 11–12)
- QA matrix (mobile/desktop browsers).
- Data correctness checks against live event pages.
- Analytics dashboard + launch playbook.

---

## 16) Success Metrics
- D1 / D7 retention.
- Avg sessions per week.
- Completion rate of first Tour weekend.
- % players who try 3+ different cars.
- Booking click-through and conversion lift.

---

## 17) Immediate Next Steps
1. Confirm exact source of truth for event data (API/feed/admin export).
2. Approve launch venue count (8, 10, or 12 tracks).
3. Approve handling style target (simcade vs more simulation).
4. Start clickable prototype with 2 cars + 1 venue + booking CTA.

