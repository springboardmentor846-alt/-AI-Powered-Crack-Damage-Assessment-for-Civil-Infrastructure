# InfraScan

**AI-Powered Crack & Damage Assessment for Civil Infrastructure**

InfraScan is a platform designed to help inspect infrastructure images, detect cracks and surface damage using AI, assess damage severity, and manage inspection results.

## Project Modules

- **Frontend:** React, TypeScript, Tailwind CSS
- **Backend:** FastAPI, Python
- **AI Module:** Computer vision and crack detection
- **Database:** PostgreSQL

## Team Responsibilities

- Frontend development and integration
- Backend APIs and database management
- AI model development and evaluation

## Project Status

Initial development and architecture setup.

**Note:** AI predictions and damage assessments must be validated before being used for real-world infrastructure decisions.

## Run with Docker

Install and start Docker Desktop, then from the repository root run:

```powershell
docker compose up --build -d
```

Open the frontend at <http://localhost:3000>, the API documentation at
<http://localhost:8000/docs>, or check API health at <http://localhost:8000/health>.
The frontend's API URL defaults to `http://localhost:8000`; set
`NEXT_PUBLIC_API_URL` before building if you access the frontend from another
host. The value is compiled into the frontend bundle, so rebuild after changing it.
For non-local deployments, set `DB_USER`, `DB_PASSWORD`, `DB_NAME`, and
`SECRET_KEY` in a root `.env` file; the Compose defaults are for local development.

To follow service logs or stop the stack:

```powershell
docker compose logs -f
docker compose down
```

PostgreSQL data and uploaded files are stored in named Docker volumes and are
preserved by `docker compose down`. To remove those volumes as well, use
`docker compose down -v`.

## Deploy a free demo on Render

1. Push this repository to GitHub.
2. In Render, choose **New → Blueprint**, connect the repository, and apply the
   `render.yaml` Blueprint.
3. Wait for the database and both web services to finish deploying. Open the
   `infrascan-frontend` service URL.

The free demo uses Render's free Postgres and web-service plans, which may sleep
when idle and have limited availability/retention. Uploaded files use the web
service's ephemeral filesystem and can be lost when the service restarts or
redeploys. Use paid plans and persistent storage for data you need to keep.
