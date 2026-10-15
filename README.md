# QuarantineCare

A working, deployable prototype for a virus quarantine and treatment facility.  
**No backend required** — all data lives in `localStorage`.

🌐 **Live Deployed Prototype**: [https://quarantine-care-lovat.vercel.app](https://quarantine-care-lovat.vercel.app)  
*(Backup/Direct Deployment URL: [https://quarantine-care-r7zx1zzlr-ronit-0173.vercel.app](https://quarantine-care-r7zx1zzlr-ronit-0173.vercel.app))*

---

## 🚀 Quick Start (Local)

```bash
# 1. Install dependencies
npm install

# 2. Run dev server
npm run dev
```

Open [http://localhost:5173](http://localhost:5173)

---

## 🏗️ Tech Stack

| Tool | Purpose |
|---|---|
| React 18 + TypeScript | UI framework |
| Vite | Build tool / dev server |
| Tailwind CSS v3 | Styling |
| React Router v6 | Client-side routing |
| Recharts | Temperature trend charts |
| Lucide React | Icons |
| localStorage | Persistence (no backend) |

---

## 👥 Demo Roles

On the login screen, select a role and enter any name (no password needed):

| Role | Default Name | Home Path |
|---|---|---|
| **Nurse** | Nurse Priya | `/nurse` |
| **Doctor** | Dr. Sharma | `/doctor` |
| **Admin** | Admin Rajesh | `/admin` |

Use **Switch Role** (top-right / sidebar) to switch roles during the demo.

---

## 🔄 Demo Flow (End-to-End)

1. **Login as Admin** → Dashboard shows 74/74 occupied
2. Click **Admit Patient** → blocked ("Facility at Full Capacity")
3. Go to **Discharge Queue** → discharge a patient to free a room
4. Return to **Admit Patient** → now admits successfully
5. **Switch to Nurse** → find the new patient in "Today's Round" (Temp Pending)
6. Tap **Record** → enter temperature, save
7. **Switch to Doctor** → new patient now appears in "Ready for Visit"
8. Tap patient → **Log Visit & Treatment** (was disabled before temp recorded)
9. Continue recording 36.x°C for 3 consecutive days
10. **Switch to Admin** → patient appears in **Discharge Queue** automatically
11. Click **Discharge** → room freed, dashboard stats update

---

## 🏥 Business Rules Enforced

- **One temperature per patient per day** — duplicate reading shows warning + "Correct reading" flow with audit log
- **Doctor visit requires today's temperature** — button disabled with explanation until nurse records temp
- **Fever threshold**: ≥ 38.0°C (100.4°F)
- **At-risk flag**: 2+ consecutive days ≥ 39.5°C
- **Fever-free streak**: consecutive calendar days < 38°C; gap or fever resets to 0
- **Ready for discharge**: streak ≥ 3 days
- **Mortality alert**: red banner when deceased/closed > 15%
- **Role-based actions**: only Nurse records temp, only Doctor logs visit/marks deceased, only Admin admits/discharges
- **Temperature validation**: 34–43°C; values outside this are rejected

---

## ☁️ Deploy to Vercel

### Option A — Vercel CLI

```bash
npm i -g vercel
vercel login
vercel --prod
```

### Option B — Vercel Dashboard (recommended)

1. Push this folder to a GitHub repository
2. Go to [vercel.com/new](https://vercel.com/new)
3. Import the repository
4. Vercel auto-detects Vite — no changes needed
5. Click **Deploy**

The `vercel.json` included in the project handles SPA routing (all paths → `index.html`).

### Build settings (auto-detected, but just in case)

| Setting | Value |
|---|---|
| Framework Preset | Vite |
| Build Command | `npm run build` |
| Output Directory | `dist` |
| Install Command | `npm install` |

---

## 📁 Project Structure

```
src/
├── types.ts                    # Domain types + pure helper functions
├── data/
│   └── store.ts                # localStorage layer + seed data generator
├── context/
│   └── AppContext.tsx           # React context (state + all actions + toasts)
├── components/
│   ├── Layout.tsx               # Sidebar (desktop) + bottom nav (mobile)
│   ├── TempChart.tsx            # Recharts temperature line chart
│   └── ToastContainer.tsx       # Toast notification system
├── pages/
│   ├── LoginPage.tsx            # Role picker login
│   ├── nurse/
│   │   ├── NurseRoundPage.tsx   # Today's round list
│   │   ├── PatientDetailPage.tsx
│   │   └── RecordTempModal.tsx  # Temp recording modal
│   ├── doctor/
│   │   ├── DoctorVisitsPage.tsx # Today's visit queue
│   │   └── DoctorPatientPage.tsx
│   └── admin/
│       ├── AdminDashboard.tsx   # Stats + outcomes
│       ├── AdmitPatientPage.tsx
│       ├── DischargeQueuePage.tsx
│       └── AllPatientsPage.tsx
└── App.tsx                      # Router + protected routes
```

---

## 🔁 Reset Demo Data

A **"Reset demo data"** button is visible at the bottom of every screen.  
It regenerates all 74 active patients + 20 historical cases from the seed generator.

---

## 📊 Seed Data Summary

| Category | Count |
|---|---|
| Active patients (rooms 1–74) | 74 |
| Fever-free streak ≥ 3 (ready) | ~5 |
| Fever-free streak = 2 | ~4 |
| Fever-free streak = 1 | ~4 |
| High fever / at-risk | ~10 |
| Historical closed cases | 20 |
| Deceased (of 20 closed) | 3 |
| Discharged cured | 17 |

---

*QuarantineCare — Demo Prototype*
