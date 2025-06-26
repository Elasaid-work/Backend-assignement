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

yaml
Copy
Edit

---

## 🚀 How to Run

> Requires [Docker](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/) installed.

1. Clone the repo and navigate into it:

```bash
git clone <your-repo-url>
cd <project-folder>
Build and start the full system:

bash
Copy
Edit
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
/service1	Golang backend (8081)
/service2	Python backend (8082)

📦 Docker Containers
🔧 service1 — Go app
Language: Golang

Dockerfile: builds Go binary and exposes on port 8081

Health endpoint: /health

🔧 service2 — Python FastAPI app
Language: Python 3.11 with pyproject.toml and uvicorn

Uses Poetry to manage dependencies

Exposes port 8082

Health endpoint: /health

🌐 nginx — Reverse Proxy
Receives all requests on port 8080

Routes to /service1 or /service2 based on path prefix

Logs all requests with timestamps

🛡️ Healthchecks
Both services include Docker healthchecks to monitor availability.

Example (for Python app):

yaml
Copy
Edit
healthcheck:
  test: ["CMD-SHELL", "python -c 'import urllib.request; urllib.request.urlopen(\"http://localhost:8082/health\")' || exit 1"]
  interval: 10s
  retries: 3
Logs show status with healthy or unhealthy using:

bash
Copy
Edit
docker ps
📜 Logs
View Nginx logs:
bash
Copy
Edit
docker exec -it nginx tail -f /var/log/nginx/access.log
View logs for all services:
bash
Copy
Edit
docker-compose logs -f
🧪 Testing Locally
Once running, test endpoints:

bash
Copy
Edit
curl http://localhost:8080/service1/
curl http://localhost:8080/service2/
Check health:

bash
Copy
Edit
curl http://localhost:8080/service1/health
curl http://localhost:8080/service2/health
📤 Deployment / Cleanup
Stop containers:
bash
Copy
Edit
docker-compose down
Rebuild clean:
bash
Copy
Edit
docker-compose down -v --remove-orphans
docker-compose up --build
🎁 Bonus Implemented
✅ Nginx access logging
✅ Healthchecks on both services
✅ Poetry-based modern Python packaging
✅ /health endpoints for monitoring
✅ Clean modular Docker structure
✅ Works with a single command: docker-compose up --build

