# FleetTwin AI

> **Tagline:** Predict. Simulate. Optimize. Deliver.

**FleetTwin AI** is an AI-assisted logistics digital twin and decision-support SaaS platform designed for small and medium logistics companies. Unlike basic GPS telematics dashboards that merely display vehicle markers, FleetTwin AI models real-world freight disruptions, simulates alternative operational responses, and optimizes dispatch decisions using deterministic, explainable financial impact algorithms.

---

## 🚀 Live Demo Scenario: Chennai–Bengaluru Delivery Disruption

The application features a built-in repeatable live demonstration scenario:
- **Truck ID:** `TN-07` (Driver: Rajesh Kumar)
- **Origin ➔ Destination:** Chennai Hub ➔ Bengaluru Electronics City (NH-48 Corridor)
- **Cargo:** 14.2 Tons of Precision Smartphone Display Panels & High-Value ICs (Foxconn Technology Group)
- **Original Expected Arrival (ETA):** 5:30 PM
- **Contractual SLA Delivery Deadline:** 6:00 PM
- **Triggered Disruption:** Severe 180-minute traffic gridlock at Sriperumbudur Toll (NH-48)
- **Updated ETA:** 8:30 PM (**2.5 hours past deadline!**)

### Decision Optimization Flow:
1. **Fleet Command Center:** Flags `TN-07` in red as **Delayed**; shipment flagged as **At Risk**.
2. **Disruption Center:** Logs the 180-minute incident with location, severity, and telemetry source.
3. **What-If Simulator:**
   - **Option A (Continue Route):** Retains 180m delay, incurs ₹3,750 late SLA penalty.
   - **Option B (Alternative Route):** Diverts via SH-58 / Ranipet bypass (+38 km detour). Incremental fuel & toll: ₹1,410.53; reduces transit delay to 66 mins; beats SLA deadline.
   - **Option C (Vehicle Reassignment):** Evaluates nearby available fleet vehicles (e.g. `KA-01`). Incurs transshipment handling cost.
4. **Intelligent Recommendation:** Recommends Option B, saving over ₹1,400+ in net business impact and preventing 114 minutes of customer delay.
5. **Manager Approval:** 1-click approval updates vehicle state in SQLite, adjusts shipment ETA, and creates an audit decision record.
6. **Connected Driver Portal:** In-cab mobile interface instantly displays the approved manager directive: *"Divert immediately to SH-58 / Ranipet Bypass Route..."*
7. **Impact Analytics:** Quantifies cumulative avoided delays and projected cost savings.

---

## 🛠️ Technology Stack

- **Frontend:**
  - React 18 with Vite
  - TypeScript
  - Tailwind CSS (Navy / Charcoal / Cyan Dark Theme)
  - Lucide React Icons
  - Recharts (Interactive financial and delay comparison charts)
  - Leaflet with OpenStreetMap (Tile caching + offline fallback)
  - React Router v6
- **Backend:**
  - Python 3.10+ / 3.14
  - FastAPI
  - Pydantic v2
  - SQLAlchemy ORM
  - SQLite Database (`fleettwin.db`)
  - Pytest (Comprehensive unit test suite)

---

## 📁 Project Structure

```
fleettwin-ai/
  ├── backend/
  │   ├── app/
  │   │   ├── api/             # REST API routers (dashboard, trucks, disruptions, simulations, drivers, analytics)
  │   │   ├── database.py      # SQLite connection & session maker
  │   │   ├── models.py        # SQLAlchemy relational schema
  │   │   ├── schemas.py       # Pydantic validation schemas
  │   │   ├── seed_data.py     # South Indian freight corridors seed & demo generator
  │   │   ├── simulation/      # Core What-If simulator, optimizer & AI explainer
  │   │   └── main.py          # FastAPI application entrypoint
  │   ├── tests/
  │   │   └── test_simulation.py # Unit tests covering all 8 specified scenarios
  │   └── requirements.txt
  ├── frontend/
  │   ├── src/
  │   │   ├── components/      # Navbar, Sidebar, FleetMap, StatCard, TruckDetailModal, AIExplanationCard
  │   │   ├── pages/           # CommandCenter, DisruptionCenter, WhatIfSimulator, DriverPortal, ImpactAnalytics, AboutPage
  │   │   ├── api.ts           # REST API client
  │   │   ├── types.ts         # TypeScript interfaces
  │   │   └── App.tsx          # Application routing & state
  │   ├── package.json
  │   ├── tailwind.config.js
  │   └── vite.config.ts
  └── README.md
```

---

## ⚡ Local Setup & Execution (Ubuntu / Linux)

### 1. Prerequisites
- Python 3.10 or higher
- Node.js 18 or higher & npm

### 2. Backend Setup
```bash
# Navigate to workspace root
cd /home/shravan-raghupathy/MyData/zMyProjsHack/Thodakkam

# Create virtual environment & install requirements
python3 -m venv backend/venv
./backend/venv/bin/pip install --upgrade pip
./backend/venv/bin/pip install -r backend/requirements.txt

# Start FastAPI backend server (Runs on port 8000)
PYTHONPATH=backend ./backend/venv/bin/uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
```

Backend API Docs (Swagger UI): `http://localhost:8000/docs`  
Healthcheck endpoint: `http://localhost:8000/api/health`

### 3. Frontend Setup
In a separate terminal window:
```bash
cd /home/shravan-raghupathy/MyData/zMyProjsHack/Thodakkam/frontend

# Install dependencies
npm install

# Start Vite development server (Runs on port 5173)
npm run dev -- --host 0.0.0.0 --port 5173
```

Open the application in your browser:
👉 **`http://localhost:5173`**

---

## 🧪 Running Automated Unit Tests

Run the full automated pytest suite covering all 8 decision engine scenarios:
```bash
cd /home/shravan-raghupathy/MyData/zMyProjsHack/Thodakkam
PYTHONPATH=backend ./backend/venv/bin/pytest backend/tests/test_simulation.py -v
```

### Verified Test Cases:
1. `test_no_disruption_scenario`: 0 min delay evaluates with zero penalty.
2. `test_traffic_delay_scenario`: Calculates accurate late-delivery business penalty.
3. `test_rerouting_scenario`: Evaluates alternative bypass corridor feasibility and savings.
4. `test_feasible_truck_reassignment`: Reassigns cargo to nearby idle vehicle with sufficient payload capacity.
5. `test_reassignment_rejected_insufficient_capacity`: Rejects reassignment when candidate vehicle capacity is lower than cargo weight.
6. `test_no_feasible_alternative`: Handles scenarios where rerouting detour exceeds delay savings.
7. `test_recommendation_changes_with_assumptions`: Verifies dynamic adaptability when fuel price or SLA penalty rate sliders are adjusted.
8. `test_correct_state_update_after_manager_approval`: End-to-end verification of database state update, driver instruction sync, and audit log.

---

## 🔄 Resetting the Demo Data
Click the **"Reset"** button in the top navigation bar at any time, or trigger it via curl:
```bash
curl -X POST http://localhost:8000/api/demo/reset
```

---

## 💼 Business Model & Pricing

| Plan | Target Fleet | Proposed Price | Key Inclusions |
|---|---|---|---|
| **Starter** | Up to 5 Trucks | ₹999 / mo | What-If Rerouting, In-Cab Driver Portal, FASTag alerts |
| **Growth** | Up to 20 Trucks | ₹2,999 / mo | Cross-dock vehicle reassignment, Custom SLA rates |
| **Pro** | Up to 50 Trucks | ₹6,999 / mo | Multi-corridor optimization, Audit trail & ROI export |
| **Enterprise** | 50+ Trucks | Custom | ERP/TMS APIs, Dedicated integration engineer |

*(All prices proposed for prototype demonstration)*
