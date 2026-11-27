# InfraScan: AI-Powered Crack & Damage Assessment for Civil Infrastructure

InfraScan is a web application that uses deep learning to detect and assess cracks and damage in civil infrastructure such as roads, bridges and buildings. Inspectors upload images, the AI models analyse them, and the results are stored and managed through a role-based system.

Built as an individual project during the **Infosys Springboard AI/ML internship**.

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
- **Frontend dashboard** for uploading images and viewing results
- **REST API** built with FastAPI
- **PostgreSQL database** for users, inspections and results
- **Dockerised deployment** for easy setup on any machine or cloud VM

---

## Tech Stack

| Layer | Technology |
|---|---|
| Backend | FastAPI (Python) |
| Database | PostgreSQL (Neon) |
| Models | YOLOv8, ResNet (Keras / PyTorch) |
| Dataset | RDD2022 (Road Damage Dataset) |
| Deployment | Docker, Azure VM (Ubuntu) |
| Model weights | Hosted on Hugging Face |

---

## Project Structure
```
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
- PostgreSQL database (local or hosted, for example Neon)
- Docker (optional, for containerised run)

### 1. Clone the repository

```bash
git clone https://github.com/IamAnujYadav/Infrascan.git
cd Infrascan
```

### 2. Set up environment variables

Create a `.env` file inside the `backend/` folder:

```env
DATABASE_URL=postgresql://user:password@host:5432/dbname
SECRET_KEY=your_secret_key
```

> Never commit your `.env` file to GitHub.

### 3. Run locally

```bash
cd backend
python -m venv venv
venv\Scripts\activate        # Windows
# source venv/bin/activate   # Linux / macOS

pip install -r requirements.txt
uvicorn app.main:app --host 0.0.0.0 --port 7860
```

Open `http://localhost:7860/docs` for the interactive API documentation.

### 4. Run with Docker

```bash
docker build -t infrascan .
docker run -p 7860:7860 --env-file .env infrascan
```

---

## Model Weights

The trained model files are not stored in this repository because of their size. They are hosted on Hugging Face:

- Repository: `Iamanujyadav/infrascan-weights`
- Files: `best_finetuned3.keras`, `resnet_crack_B.keras`

Download them and place them in `backend/ai/weights/`.

---

## Usage

1. Open the live link or run the app locally.
2. **Sign up** to create an account. You start as an **Inspector**.
3. Upload an infrastructure image to get the crack and damage analysis.
4. **Admin users** can log in with the admin email and change the role of any user.

---

## Deployment

The application is deployed as a Docker container on an Azure Ubuntu VM, with Neon serving as the managed PostgreSQL database.

---

## Author

**Anuj Yadav**
B.Tech CSE, JECRC University, Jaipur
GitHub: [IamAnujYadav](https://github.com/IamAnujYadav)

Designed and built the complete project: AI models, backend, frontend and deployment.

---

## Future Improvements

- Severity scoring for detected damage
- Downloadable inspection reports (PDF)
- Support for video and drone footage
- Geo-tagging of inspected locations

---

## Acknowledgements

- Infosys Springboard for the internship and project specification
- Crack500
- RDD2022 dataset authors
- Ultralytics YOLOv8

---
