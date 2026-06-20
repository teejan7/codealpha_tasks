# Web Server using Docker — CodeAlpha DevOps Internship

## Overview
A personal portfolio website containerized and served using Docker and Nginx. This task covers Docker basics — building images, running containers, port mapping, and monitoring container health.

## Tech Stack
- Docker & Docker Compose
- Nginx (alpine)
- HTML/CSS

## Project Structure
```
Task_DockerWebServer/
├── app/
│   └── index.html
├── Dockerfile
├── docker-compose.yml
├── screenshots/
└── README.md
```

## Dockerfile
```dockerfile
FROM nginx:alpine
COPY app/ /usr/share/nginx/html/
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

## How to Run
```bash
cd DevOps/Task_DockerWebServer
docker-compose up --build
```
Visit: `http://localhost:8080`

To stop:
```bash
docker-compose down
```

## Screenshots
1. `01_website_live_browser.png` — Website running at localhost:8080
2. `02_docker_desktop_containers.png` — Container running in Docker Desktop
3. `03_dockerfile_and_terminal_logs.png` — Build/run terminal logs
4. `04_container_logs_detailed.png` — Nginx logs showing live requests

## What I Learned
- How Dockerfiles build reproducible container images
- Difference between an image and a running container
- Port mapping between host and container
- Monitoring container logs and health

## Status
✅ Completed and verified working

---
**Intern:** Teejan Tee Pee | **Domain:** DevOps | **CodeAlpha Internship (June–July 2026)**