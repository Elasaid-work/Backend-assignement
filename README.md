# 🐳 DevOps Reverse Proxy Project — Docker Compose + Nginx + FastAPI + Go

This project sets up a containerized microservice system with:

- A **FastAPI** Python app (service 2)
- A **Golang** web app (service 1)
- An **Nginx reverse proxy** that routes requests to both services
- All orchestrated via **Docker Compose**

---

## 📁 Project Structure

.
├── docker-compose.yml
├── nginx/
│ ├── nginx.conf
│ └── Dockerfile
├── service_1/
│ ├── Dockerfile
│ └── main.go
├── service_2/
│ ├── Dockerfile
│ ├── pyproject.toml
│ └── app.py
└── README.md

---

## 🚀 How to Run

> Requires [Docker](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/) installed.

1. Clone the repo and navigate into it:

```bash
git clone <your-repo-url>
cd <project-folder>

Build and start the full system:
docker-compose up --build
Test the services:
FastAPI (Python) app: http://localhost:8080/service2/
Go app: http://localhost:8080/service1/
Health endpoints:
http://localhost:8080/service1/health
http://localhost:8080/service2/health

🌐 Routing Overview
Nginx reverse proxy handles routing using path prefixes:
URL Path	Proxied to
/service1	Golang backend (8001)
/service2	Python backend (8002)

📦 Docker Containers
🔧 service1 — Go app
Language: Golang
Dockerfile: builds Go binary and exposes on port 8001
Health endpoint: /health
🔧 service2 — Python FastAPI app
Language: Python 3.11 with pyproject.toml and uvicorn
Uses Poetry to manage dependencies
Exposes port 8002
Health endpoint: /health


🌐 nginx — Reverse Proxy
Receives all requests on port 8080
Routes to /service1 or /service2 based on path prefix
Logs all requests with timestamps


🛡️ Healthchecks
Both services include Docker healthchecks to monitor availability.

Example (for Python app):
healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost:8002/health"]
  interval: 10s
  retries: 3
Logs show status with healthy or unhealthy using:
docker ps


📜 Logs
View Nginx logs:
docker exec -it nginx tail -f /var/log/nginx/access.log
View logs for all services:
docker-compose logs -f


🧪 Testing Locally
Once running, test endpoints:
curl http://localhost:8080/service1/
curl http://localhost:8080/service2/

Check health:
curl http://localhost:8080/service1/health
curl http://localhost:8080/service2/health

📤 Deployment / Cleanup
Stop containers:
docker-compose down
Rebuild clean:
docker-compose down -v --remove-orphans
docker-compose up --build


🎁 Bonus Implemented
✅ Nginx access logging
✅ Healthchecks on both services
✅ Poetry-based modern Python packaging
✅ /health endpoints for monitoring
✅ Clean modular Docker structure
✅ Works with a single command: docker-compose up --build

