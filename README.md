# Ward Ledger

Temperature tracking and discharge management for a virus quarantine and treatment facility.

Ward Ledger is a working prototype built for a Technical Product Manager assessment. It covers the full patient path: daily temperature entry, doctor visits, discharge recommendation, discharge and room release, admissions, and mortality tracking.

**Live demo:** `https://<your-username>.github.io/ward-ledger/`

## Run it

It is a single static file with no build step and no dependencies.

- Open `index.html` in any modern browser, or
- Enable GitHub Pages (Settings → Pages → Deploy from branch → `main` / root) and open the link above.

The page loads the IBM Plex fonts from Google Fonts. Without an internet connection it falls back to system fonts.

## Try it

The app opens on the sign-in page. Choose a role and a name, then sign in. Each role lands on its own dashboard and has its own URL.

| Role | Lands on | Can do |
|------|----------|--------|
| Nurse | Today's round (`#/nurse`) | Record and correct temperatures, send and read notes |
| Doctor | Rounds (`#/doctor`) | Visit patients whose temperature is in, recommend discharge, record a death |
| Admin | Overview (`#/admin`) | Complete discharges, admit from the waiting list, view occupancy, outcomes and the activity log |

Sign out, or use the browser Back button, to return to the login page. A fresh load always starts at login.

The demo starts with 74 occupied rooms, a waiting list and a day already under way. Use **Start next day** and **Reset demo data** at the bottom of any screen.

### Things worth testing

- **Temperature entry (nurse):** enter `38.6`. `45` is rejected. `40.5` asks for a second tap to confirm.
- **Correction (nurse):** tap Correct on a measured patient. A reason is required and the first reading is voided but kept in history.
- **Visit lock (doctor):** a patient who is waiting for a temperature cannot be visited.
- **Discharge (doctor, then admin):** recommend a "Discharge-eligible" patient, then sign in as admin and complete the discharge. The room is freed.
- **Capacity (admin):** admission is blocked at 74 of 74 rooms. Discharge someone, then admit from the waiting list.
- **Mortality alert (doctor):** record two deaths, then open Overview. Mortality moves from 13.3% to above 15%.
- **Notes:** send an urgent note as one role, then sign in as the receiving role to read it.

## Rules the prototype enforces

**Temperature**
- Unit is °C with one decimal. Accepted range is 34.0 to 42.0.
- Fever is 38.0 or higher. Critical is 40.0 or higher.
- A second tap is needed to confirm when a reading is below 35.5, or differs by 1.5 or more from the last reading within 2 days.
- One active reading per patient per calendar day. A second entry is a correction: reason required, nothing is deleted.

**Visits and discharge**
- A visit can be saved only if today's temperature exists. One visit per patient per day.
- A patient is eligible for discharge after 3 consecutive calendar days with a reading, all below 38.0. A missing day breaks the count.
- The doctor recommends, the admin completes. Completing a discharge frees the room.

**Capacity and outcomes**
- 74 rooms, one patient per room.
- Mortality = deaths / (recovered + deaths). Benchmark: 85% success, 15% mortality. A notice shows from 13% and a banner above 15%.

**Platform**
- Role-based actions: nurses capture data, doctors make clinical decisions, admins handle rooms and outcomes.
- Every write is stored in the activity log with user, role and time.
- Notes go to one team, are limited to 280 characters, and cannot be edited or deleted.

## Prototype limits

- All data lives in the browser's localStorage. There is no server, so role enforcement is in the interface only. The spec calls for server-side enforcement.
- There is no password. Sign-in is a role and name picker for demo purposes.
- The 15-minute idle session timeout from the spec is not implemented.
- Discharge cannot be reversed, and there is no clinical override for the 3-day rule (both are v1 decisions in the spec).

## Files

| File | Purpose |
|------|---------|
| `index.html` | The whole application: markup, styles and logic |
| `README.md` | This file |

## Sprint scope (from the assessment)

Features A, B, D, F, G, H, I and J: 8.3 points of effort and a value of 22, which is the maximum value achievable within the 10-point capacity.
  give this in proper format
