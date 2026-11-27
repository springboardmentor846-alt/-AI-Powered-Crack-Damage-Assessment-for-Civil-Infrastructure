# InfraScan

**AI-Powered Crack & Damage Assessment for Civil Infrastructure**

InfraScan is a full-stack civil infrastructure inspection platform that detects surface cracks from field and drone photography, extracts pixel-calibrated geometric measurements, scores structural severity and maintenance priority, and manages end-to-end engineering sign-offs, work orders, and PDF/CSV audit reports.

> [!IMPORTANT]
> **Engineering Disclaimer:** Automated computer-vision crack detections, severity scores, and pixel-based dimension estimates are decision-support aids only. All findings must be reviewed and signed off by a qualified structural engineer before closing inspections or dispatching maintenance interventions.

---

## Key Features

- **AI Crack Segmentation & Geometry (`ai-model/`):**
  - YOLOv11-seg (`yolo11n-seg.pt` / fine-tuned `best.pt`) with an automatic low-memory OpenCV morphological segmentation fallback for constrained cloud containers.
  - Medial-axis skeletonization and distance-transform measurements via `scikit-image` and `scipy`: crack length (`px`), average/max width (`px`), area (`px`), crack density (`%`), and orientation angle/classification (`Horizontal`, `Vertical`, `Diagonal`).
  - Explainable AI (XAI) attention heatmaps, binary segmentation masks, and bounding-box overlays.
- **Image Quality & Preprocessing:** Automated image quality checks and CLAHE contrast enhancement before inference.
- **Civil Infrastructure Management (`backend/` & `frontend/`):**
  - **Infrastructure Assets & Map:** Registry of bridges, tunnels, dams, and highways with Leaflet geospatial mapping and condition status tracking.
  - **Inspections & Engineer Review:** Multi-step inspection creation, AI analysis viewer (Original Photo, Crack Bounding Boxes, Segmentation Mask, Attention Heatmap), and engineer sign-off (`Approved` / `Requires Reinspection`).
  - **Maintenance Work Orders:** Automated or manual work-order dispatch with priority filtering and CSV export.
  - **Audit Reports:** Auto-compiled inspection reports with downloadable PDF (`reportlab`) and CSV exports.
  - **Damage Analytics & Model Governance:** Live analytics computed from backend inspection/asset records, plus AI model version benchmarks.
- **Persistent Cloud Storage:** PostgreSQL (`DATABASE_URL`) with SQLite fallback for local development, plus database-backed media persistence (`media_files`) so uploaded photos, masks, heatmaps, and PDF reports survive ephemeral container restarts.

---

## Architecture & Project Structure

```text
infrascan/
├── frontend/                   # Next.js 16 (App Router), React 19, TypeScript, Tailwind CSS
│   ├── app/                    # Pages & same-origin /api/proxy/[...path] route
│   ├── components/             # AnalysisView, AnalyticsView, Assets, Reports, Work Orders, Auth
│   └── lib/api/                # Typed API client & service layer
├── backend/                    # FastAPI, SQLAlchemy, Pydantic, ReportLab
│   ├── app/
│   │   ├── api/v1/             # auth, assets, inspections, maintenance, models, reports, notifications, preferences
│   │   ├── core/               # Config (CORS, JWT, DATABASE_URL, UPLOAD_DIR) & security
│   │   ├── db/                 # SQLAlchemy engine/session & seed initialization
│   │   ├── models/             # ORM models (User, Asset, Inspection, MaintenanceWorkOrder, InspectionReport, MediaFile, etc.)
│   │   └── services/           # AI pipeline bridge, preprocessing, PDF generator, DB media storage
│   └── tests/                  # FastAPI API tests & sample concrete images
├── ai-model/                   # Computer vision & crack geometry package (infrascan_ai)
│   ├── infrascan_ai/           # yolo_bridge, geometry, pipeline, severity, priority, progression, visualization
│   └── src/                    # Model checkpoints & training/evaluation scripts
├── Dockerfile                  # Production backend + AI container (CPU PyTorch, OpenCV headless, port 10000)
├── docker-compose.yml          # Local multi-container setup
└── render.yaml                 # Render.com Blueprint (PostgreSQL + Docker backend + Node frontend)
```

---

## Tech Stack

| Layer | Technologies |
|---|---|
| **Frontend** | Next.js 16, React 19, TypeScript, Tailwind CSS, Recharts, Leaflet, Lucide Icons |
| **Backend** | Python 3.11+, FastAPI, Uvicorn, SQLAlchemy 2.0, Pydantic v2, ReportLab, Python-JOSE, Passlib |
| **AI / Vision** | PyTorch (CPU), Ultralytics YOLOv11-seg, OpenCV (`opencv-python-headless`), NumPy, scikit-image, SciPy |
| **Database** | PostgreSQL (production via Render) / SQLite (local fallback) |
| **Deployment** | Docker, Render Blueprint (`render.yaml`) |

---

## Getting Started (Local Development)

### Prerequisites
- Python `3.11+`
- Node.js `20+` (or `pnpm` / `npm`)
- Docker Desktop (optional, for containerized run)

### Option 1: Docker Compose

```bash
docker compose up --build
```

### Option 2: Manual Local Setup

#### 1. Backend & AI Module
```bash
cd backend
python -m venv venv
# Windows PowerShell:
.\venv\Scripts\Activate.ps1
# macOS/Linux:
# source venv/bin/activate

pip install -r requirements.txt
pip install -r ../ai-model/requirements.txt
```

Run the FastAPI server from `backend/` (with `ai-model` on `PYTHONPATH`):
```bash
# Windows PowerShell:
$env:PYTHONPATH="$PWD;$PWD\..\ai-model"
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```
- API Docs: `http://localhost:8000/docs`
- Health Check: `http://localhost:8000/health`

#### 2. Frontend
```bash
cd frontend
npm install
npm run dev
```
- App URL: `http://localhost:3000`
- Set `NEXT_PUBLIC_API_URL=http://localhost:8000` in `frontend/.env.local` if connecting to a custom backend URL.

---

## Cloud Deployment (Render.com)

This repository includes a complete [`render.yaml`](render.yaml) Blueprint:
- **`infrascan-db`**: Managed PostgreSQL database (free tier)
- **`infrascan-backend`**: Docker web service built from the root [`Dockerfile`](Dockerfile)
- **`infrascan-frontend`**: Next.js web service with a server-side `/api/proxy/[...path]` route to avoid browser CORS issues

To deploy:
1. Push your branch to GitHub.
2. In the [Render Dashboard](https://dashboard.render.com/blueprints), create or **Manual sync** the Blueprint from `render.yaml`.
3. Ensure `NEXT_PUBLIC_API_URL` on `infrascan-frontend` points to your `infrascan-backend` service host/URL.

---

## Demo Credentials

The database seeds default engineering accounts on first startup:

| Role | Email | Password |
|---|---|---|
| Civil Engineer | `elena.rostova@infrastructure.gov` | `password123` |
| Infrastructure Manager | `jordan.smith@infrastructure.gov` | `password123` |
| Administrator | `david.kalu@infrastructure.gov` | `password123` |

---

## Core API Endpoints

- `GET /health` — Service, database, and storage health check
- `POST /api/v1/auth/login` — Authenticate and receive JWT bearer token
- `GET /api/v1/assets` — List infrastructure assets
- `GET, POST /api/v1/inspections` — List or create inspections
- `POST /api/v1/inspections/preprocess` — Image quality assessment & CLAHE preprocessing
- `POST /api/v1/inspections/{id}/analyze` — Run crack segmentation, geometry extraction, heatmap generation, and auto-compile report
- `POST /api/v1/inspections/{id}/review` — Submit civil engineer sign-off
- `GET, POST /api/v1/work-orders` — Manage maintenance work orders & CSV export
- `GET /api/v1/reports` & `GET /api/v1/reports/{id}/download` — View & download official PDF/CSV audit reports

---

## License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.
