# DevOps Intern Assessment R1 - Project Overview

## Goal
Deploy Directus (Headless CMS) on AWS EC2 using Docker Compose and verify the deployment with a GitLab CI/CD pipeline.

## High-level Workflow
1. Provision an AWS EC2 instance (Ubuntu) and allow access to port 8055.
2. Deploy Directus + PostgreSQL with Docker Compose on the VM.
3. Configure Directus to store assets in S3.
4. Create a GitLab CI/CD pipeline that validates the compose file, checks service health, captures the homepage, and generates a deployment report.
5. Collect artifacts and screenshots as proof of deployment and pipeline success.

## Architecture Overview
- **Compute:** AWS EC2 (Ubuntu).
- **Application:** Directus on port `8055`.
- **Database:** PostgreSQL 15 in a container.
- **Storage:**
  - Postgres data persists via Docker volume `db_data`.
  - Directus assets stored in S3 bucket `directus-vega-hari-2026` (region `ap-south-1`).

## Deployment Configuration (Docker Compose)
File: `docker-compose.yml`

### Services
- **db**
  - Image: `postgres:15`
  - Container: `directus-db`
  - Env: `POSTGRES_USER`, `POSTGRES_PASSWORD`, `POSTGRES_DB`
  - Volume: `db_data:/var/lib/postgresql/data`

- **directus**
  - Image: `directus/directus:latest`
  - Container: `directus-app`
  - Ports: `8055:8055`
  - Depends on: `db`
  - Env:
    - `KEY`, `SECRET` (Directus security)
    - `ADMIN_EMAIL`, `ADMIN_PASSWORD` (initial admin)
    - DB connection vars (`DB_CLIENT`, `DB_HOST`, `DB_PORT`, `DB_DATABASE`, `DB_USER`, `DB_PASSWORD`)
    - S3 storage vars (`STORAGE_S3_*`)

### Storage and Secrets
- S3 credentials are provided via environment variables:
  - `STORAGE_S3_KEY`, `STORAGE_S3_SECRET`
- No secrets are hardcoded in the repo.

## CI/CD Pipeline (GitLab)
File: `.gitlab-ci.yml`

### Pipeline Stages
1. **validate**
   - Job: `validate_config`
   - Purpose: Basic validation of `docker-compose.yml`
   - Actions: `ls -la` and `cat docker-compose.yml`

2. **test**
   - Job: `health_check`
     - Checks HTTP availability for Directus via `curl -I --fail`
   - Job: `capture_homepage`
     - Downloads Directus homepage HTML to `directus_homepage.html`
     - Stores artifact

3. **report**
   - Job: `generate_report`
   - Writes a short summary to `deployment_report.md`
   - Stores artifact

### CI Variables
- `DIRECTUS_URL`: `http://13.232.23.3:8055`

## Artifacts and Evidence
- `artifacts_1/directus_homepage.html` (captured homepage)
- `artifacts_0/deployment_report.md` (pipeline report)
- Screenshots (examples):
  - `Directus running.png`
  - `pipelines.png`, `jobs.png`
  - `gitlab repo.png`
  - `ec2 instance.png`
  - `security group.png`
  - `S3 bucket .png`
  - `artifacts.png`

## Validation Summary
- YAML syntax and indentation are correct in `.gitlab-ci.yml` and `docker-compose.yml`.
- Pipeline validates connectivity and produces artifacts successfully.

## How to Run (Manual)
1. SSH into the EC2 instance.
2. Ensure Docker and Docker Compose are installed.
3. From the project directory:
   - `docker compose up -d`
4. Open `http://<EC2_PUBLIC_IP>:8055` in the browser.

## Notes
- Admin credentials are set via environment variables in `docker-compose.yml`.
- Directus assets are stored on S3; DB data persists through `db_data` volume.

## Optional Enhancements
- Add `docker compose config` validation in CI.
- Add healthcheck to `directus` service.
- Move sensitive defaults (admin password) to GitLab CI/CD variables or `.env`.
