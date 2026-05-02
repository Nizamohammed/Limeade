# Limeade

Built by our team at DemonHacks 2026 — a real-time safety system that 
warns scooter riders about nearby vehicles, pedestrians, and road hazards 
using nothing but their phone camera.

---

## How It Works

The mobile app streams live camera frames over WebSocket to a Python 
backend. Every frame runs through two YOLOv8 models — one for vehicles 
and pedestrians, one for potholes and road hazards — combined with 
Farneback optical flow and IoU-based tracking to compute a danger score. 
When a threat crosses the threshold, Claude Haiku returns a structured 
assessment with an urgency level and recommended rider action. The phone 
immediately fires a graded haptic pattern and speaks the warning aloud. 
Every detection above threshold is written to PostgreSQL with GPS 
coordinates and shown on a live web dashboard.

---

## Team

| Name | GitHub |
|------|--------|
| Nizam Mohammed | [@NizamMohammed9](https://github.com/NizamMohammed9) |
| Brandon Kong | [@brandon-kong](https://github.com/brandon-kong) |
| Joseph Alemu | [@JosephSAlemu](https://github.com/JosephSAlemu) |
| Paolo Lambre | [@plambre0](https://github.com/plambre0) |
| Art Chen-Wei | — |
| visualyuu | — |

---

## Stack

| Layer | Technology |
|-------|-----------|
| Mobile | Expo React Native, expo-camera, expo-location, expo-speech |
| Backend | Python, FastAPI, asyncio, WebSockets |
| CV | YOLOv8 (Ultralytics), OpenCV, NumPy |
| AI | Anthropic Claude Haiku (`claude-haiku-4-5-20251001`) |
| Database | PostgreSQL 17, SQLAlchemy 2.0 async, asyncpg |
| Web | Vite, React 19, TypeScript, Leaflet, TanStack Query |
| Infra | Docker, pnpm monorepo, Turborepo |

---

## Running Locally

**Backend**
```bash
cd apps/api
cp .env.example .env   # add ANTHROPIC_API_KEY
docker compose -f ../../infra/docker-compose.yml up -d
pip install -r requirements.txt
uvicorn main:app --reload --port 8000
```

**Mobile**
```bash
cd apps/app
pnpm install
npx expo start
```

**Web Dashboard**
```bash
cd apps/web
pnpm install
pnpm dev
```

---

## What's Next

The hackathon system is the foundation for ongoing research targeting 
a fully edge-native deployment. Planned directions include:

**On-Device Reasoning** — replacing the cloud Claude Haiku API call 
with a distilled on-device model trained on Haiku's threat assessments. 
Target is >90% agreement with Haiku's decisions at under 5ms inference 
time, compared to the current ~500ms API round trip.

**Adaptive Detection Policy** — an RL-based contextual bandit that 
learns to allocate compute dynamically based on riding context such as 
speed, time of day, and location density, rather than running full 
inference on every frame unconditionally.

**Edge Hardware Deployment** — migrating the entire pipeline from a 
phone + cloud backend setup to a Jetson Orin Nano mounted on the 
scooter, targeting sub-200ms end-to-end latency with no cloud 
dependency.

**Crowdsourced Hazard Mapping** — expanding the existing GPS hazard 
logging into a privacy-preserving aggregated heatmap, where no raw 
imagery is stored and individual rider routes cannot be reconstructed.


---

## Note

This is a clone of the original hackathon repository 
([Lime_Scooter_Safety](https://github.com/plambre0/Lime_Scooter_Safety)). 
This repo represents my continuation of the project as I work on 
further developing and improving the system beyond the hackathon version.