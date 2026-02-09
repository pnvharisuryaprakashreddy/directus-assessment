# 🚀 Directus Deployment on AWS with Docker & CI/CD

This repository contains a complete **DevOps implementation** of **Directus (Headless CMS)** deployed on **AWS EC2** using **Docker Compose**, with automated validation using **GitLab CI/CD** and **GitHub Actions**.

---

## 📌 Project Overview

- **Application**: Directus (Headless CMS)
- **Cloud Provider**: AWS
- **Compute**: EC2 (Ubuntu 22.04)
- **Containerization**: Docker & Docker Compose
- **Database**: PostgreSQL 15
- **Storage**: AWS S3 (media assets)
- **CI/CD**: GitLab CI/CD + GitHub Actions
- **Application Port**: 8055

---

## 🌐 Live Deployment

- **Directus URL**:  
  http://13.232.23.3:8055

- **Admin Panel**:  
  http://13.232.23.3:8055/admin

---

## 🏗️ Architecture Overview

User
↓
AWS EC2 (Ubuntu 22.04)
↓
Docker Compose
├── Directus (Port 8055)
└── PostgreSQL (Internal Network)
↓
AWS S3 (Media Storage)

---

## 🐳 Docker Setup

The deployment uses Docker Compose with the following services:

- **directus**
  - Runs the Directus application
  - Exposed on port `8055`

- **db**
  - PostgreSQL 15 database
  - Persistent storage using Docker volumes

---

## 🔁 CI/CD Implementation

### GitLab CI/CD
The GitLab pipeline includes the following stages:
- Validate Docker Compose configuration
- Perform HTTP health check on Directus
- Capture homepage HTML as artifact
- Generate deployment report

Artifacts generated:
- `directus_homepage.html`
- `deployment_report.md`

### GitHub Actions
GitHub Actions is used for additional CI validation:
- Verifies Directus availability
- Captures homepage
- Uploads artifacts automatically

---

## 📸 Deployment Evidence

### Directus Running
![Directus Running](screenshots/Directus%20running.png)

### AWS EC2 Instance
![EC2 Instance](screenshots/ec2%20instance.png)

### Security Group Configuration
![Security Group](screenshots/security%20group.png)

### AWS S3 Bucket
![S3 Bucket](screenshots/S3%20bucket.png)

### GitLab Repository
![GitLab Repo](screenshots/gitlab%20repo.png)

### CI/CD Pipelines
![Pipelines](screenshots/pipelines.png)

### CI/CD Jobs
![Jobs](screenshots/jobs.png)

### Pipeline Artifacts
![Artifacts](screenshots/artifacts.png)

---

## 📂 Repository Structure

directus-assessment/
├── docker-compose.yml
├── README.md
├── screenshots/
│ ├── Directus running.png
│ ├── ec2 instance.png
│ ├── security group.png
│ ├── pipelines.png
│ ├── jobs.png
│ ├── artifacts.png
│ └── S3 bucket.png

---

## ✅ Final Status

- ✔ Directus is publicly accessible
- ✔ Dockerized deployment completed
- ✔ CI/CD pipelines passing
- ✔ Artifacts generated successfully
- ✔ Screenshots provided as proof

---

## 👨‍💻 Author

**P N V Hari Surya Prakash Reddy**  
DevOps Intern Candidate
