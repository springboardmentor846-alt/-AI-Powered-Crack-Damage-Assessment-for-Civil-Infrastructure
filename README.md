# InfraScan: AI-Powered Crack & Damage Assessment for Civil Infrastructure

InfraScan is a web application that uses deep learning to detect and assess cracks and damage in civil infrastructure such as roads, bridges, and buildings. Inspectors upload images, the AI models analyse them, and the results are stored and managed through a role-based system.

Built as part of the **Infosys Springboard AI/ML Internship** (Team 4).

**Live demo:** https://infrascan-nine.vercel.app/

---

## Features

- **Modular AI pipeline** with separate modules for:
  - Detection of cracks and damage (YOLOv8 trained on RDD2022)
  - Classification (fine-tuned ResNet)
  - Segmentation (masks and overlays)
  - Geometry measurement
  - Severity assessment
  - Priority ranking
  - Image quality checks
- **Annotated outputs** (annotated images, masks, overlays) and **inspection reports**
- **Role-based access control**
  - New accounts start with the **Inspector** role by default
  - Only an **Admin** can change user roles, so not everyone who signs up can make changes to the site
- **Frontend dashboard** for uploading images, viewing interactive damage maps, comparing before-and-after inspections, and managing roles
- **REST API** built with FastAPI
- **PostgreSQL database** for users, inspections, and results
- **Dockerised deployment** for easy setup on any machine or cloud VM

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React, Vite, TypeScript, Tailwind CSS |
| Backend | FastAPI (Python) |
| Database | PostgreSQL (Neon) |
| Models | YOLOv8, ResNet (Keras / PyTorch) |
| Dataset | RDD2022 (Road Damage Dataset) |
| Deployment | Vercel (Frontend), Docker & Azure VM (Backend) |
| Model weights | Hosted on Hugging Face |

---

## Project Structure

```text
Infrascan/
├── backend/
│   ├── ai/
│   │   ├── core/
│   │   ├── modules/
│   │   │   ├── classification/
│   │   │   ├── detection/
│   │   │   ├── geometry/
│   │   │   ├── priority/
│   │   │   ├── quality/
│   │   │   ├── segmentation/
│   │   │   └── severity/
│   │   └── weights/          # model weights (downloaded separately)
│   ├── app/
│   │   ├── api/              # API routes
│   │   ├── db/               # database models and connection
│   │   ├── schemas/          # request / response schemas
│   │   ├── security/         # authentication and role-based access
│   │   └── services/
│   ├── scripts/
│   ├── storage/              # runtime data (uploads, results, reports), not in Git
│   └── tests/
│       ├── ai/
│       └── app/
├── frontend/
│   ├── public/
│   └── src/
│       ├── api/
│       ├── assets/
│       ├── components/
│       └── context/
├── Dockerfile
└── README.md
```

---

## Getting Started

### Prerequisites

- Python 3.10+
- Node.js 18+
- PostgreSQL database (local or hosted, for example Neon)
- Docker (optional, for containerised run)

### 1. Clone the repository

```bash
git clone https://github.com/springboardmentor846-alt/-AI-Powered-Crack-Damage-Assessment-for-Civil-Infrastructure.git
cd -AI-Powered-Crack-Damage-Assessment-for-Civil-Infrastructure
git checkout Pranali
```