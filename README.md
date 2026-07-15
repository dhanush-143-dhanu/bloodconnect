# 🩸 BloodConnect — Emergency Blood Donor Finder

A location-aware web app that connects patients and hospitals with nearby, compatible blood donors in real time — no backend server required. It runs entirely as static files, so it deploys directly to **GitHub Pages** for free.

**Live demo pages:** `index.html` (home) · `find-donors.html` · `register.html` · `emergency.html` · `dashboard.html`

---

## ✨ What makes this build stronger than a basic donor list

| Feature | Why it matters |
|---|---|
| **Real geolocation, not text search** | Uses the browser's Geolocation API + the Haversine formula to compute true distance in km, not a matching city/area string. |
| **Real blood-type compatibility matrix** | O- is matched as a universal donor to every group, AB+ as universal recipient, etc. — not just "same group" matching. |
| **Live interactive map** | Donors and the requester's radius are plotted on an OpenStreetMap/Leaflet map, colour-coded by availability. |
| **Emergency broadcast simulation** | Raising a request notifies every compatible, in-range, available donor at once (staggered toasts + real browser `Notification` API), then simulates "first donor to accept wins." |
| **Urgency levels + live countdown** | Critical / Moderate / Low requests, with a running response-time timer — mirrors real triage. |
| **Donor dashboard with digital ID + QR code** | Each donor gets a scannable QR donor card, an availability toggle, and gamified badges (First Drop → BloodConnect Legend) that unlock with donation count. |
| **Installable PWA** | `manifest.json` lets a phone "Add to Home Screen" so it behaves like a native app. |
| **Zero backend** | All data lives in `localStorage` (`js/storage.js`), seeded with demo donors on first load — works fully offline after the first visit, and needs no database or hosting cost. |

> **Note on the demo "database":** Because this is a static site (by design, so it's free to host on GitHub Pages), donor and request data is stored in each visitor's own browser `localStorage`, not a shared server database. For a production version, swap `js/storage.js` for calls to a real backend (Firebase, Supabase, or your own API) — every other file already calls through that one module, so it's a single-file change.

---

## 📁 Project structure

```
BloodConnect/
├── index.html          Landing page (hero, live stats, about, how it works, blood banks, contact)
├── register.html        Donor sign-up with live location capture
├── find-donors.html     Search + interactive map + compatibility filtering
├── emergency.html       Emergency broadcast + simulated accept flow
├── dashboard.html        Donor login, digital ID/QR card, badges, donation history
├── manifest.json         PWA manifest
├── css/
│   └── style.css         Design tokens + all component styles
└── js/
    ├── storage.js        localStorage "database" layer
    ├── geo.js             Geolocation + Haversine distance
    ├── matching.js        Blood-type compatibility matrix
    ├── notify.js          Toasts + browser Notification API
    └── seed.js            Demo donor data generator
```

---

## 🚀 Run it locally

No build step, no `npm install`. Just serve the folder:

```bash
cd BloodConnect
python3 -m http.server 8080
# then open http://localhost:8080
```

(Opening `index.html` directly by double-clicking also works, but some browsers restrict Geolocation on `file://` URLs — a local server avoids that.)

---

## ☁️ Deploy to GitHub Pages (step by step)

1. **Create a new GitHub repository** (e.g. `bloodconnect`), and don't initialize it with a README (you already have one here).
2. **Push this project** to it:
   ```bash
   cd BloodConnect
   git init
   git add .
   git commit -m "Initial commit: BloodConnect emergency blood donor finder"
   git branch -M main
   git remote add origin https://github.com/<your-username>/bloodconnect.git
   git push -u origin main
   ```
3. On GitHub, open your repo → **Settings → Pages**.
4. Under **Build and deployment → Source**, choose **Deploy from a branch**.
5. Under **Branch**, choose `main` and folder `/ (root)`, then **Save**.
6. Wait ~1 minute, then GitHub shows your live URL:
   ```
   https://<your-username>.github.io/bloodconnect/
   ```
7. Open that link — the site (including geolocation, the map, and emergency broadcast demo) works exactly as it does locally, since it's fully static.

---

## 🔧 Ideas to extend further

- Replace `js/storage.js` with real API calls to persist donors/requests across devices.
- Add SMS/email delivery via a service like Twilio or EmailJS in `js/notify.js`.
- Add a hospital-only login role with blood-bank inventory counts.
- Add multi-language support (e.g. English/Hindi) for wider reach.
- Add donor eligibility screening questions to `register.html` before allowing sign-up.

---

## ⚠️ Disclaimer

This is a demo/educational project. It does not replace verified medical infrastructure, and blood donation eligibility should always be confirmed by a licensed blood bank or hospital.
