# Uptime Kuma Web API Example

A **REST API wrapper for Uptime Kuma** application published as a **sample deployment project for [Nife.io](https://nife.io)**.

This repository demonstrates how to run a lightweight Uptime Kuma API wrapper locally and deploy it on [Nife.io](https://nife.io). It is intended as a practical sample for testing API deployment workflows and showcasing a straightforward REST API deployment path.

## Overview

This project is a basic REST API application that wraps the Uptime Kuma monitoring system with multi-user support, JWT authentication, and SQLite database integration. It is designed to be small enough for learning and deployment experiments while still reflecting the conventions of a real API application.[3]

If you want a simple API sample to test deployment on [Nife.io](https://nife.io), this repository is a good starting point.

## Features

| Feature | Description |
| --- | --- |
| REST API | Complete REST API wrapper for Uptime Kuma |
| JWT Authentication | Secure token-based authentication system |
| Multi-user support | Multiple user management with SQLite database |
| Swagger documentation | Auto-generated interactive API documentation |
| Docker support | Includes `Dockerfile` for containerized execution |
| Multi-architecture | Supports amd64 and arm64 architectures |
| Deployment-ready setup | Can be deployed using Git-based workflows |
| Nife.io sample use case | Suitable as a reference project for [Nife.io](https://nife.io) deployments |

## Tech Stack

| Technology | Purpose |
| --- | --- |
| Python | Runtime environment |
| Flask | Web framework |
| SQLite | Database layer |
| JWT | Authentication mechanism |
| Docker | Container packaging |
| Nife.io | Deployment platform |

## Prerequisites

Before running the project locally, make sure the following are installed.

| Requirement | Notes |
| --- | --- |
| Python | Version 3.7+ recommended |
| pip | Python package manager |
| Docker (optional) | For containerized deployment |
| Git | Required to clone the repository |

## Getting Started

### Clone the repository

```bash
git clone https://github.com/nifetency/uptime-kuma-web-api.git
cd Uptime-Kuma-Web-API
```

### Create a virtual environment (recommended)

```bash
python -m venv venv
```

**Activate the virtual environment:**

- **On Windows:**
  ```bash
  venv\Scripts\activate
  ```

- **On macOS/Linux:**
  ```bash
  source venv/bin/activate
  ```

### Install dependencies

```bash
pip install -r requirements.txt
```

### Set up environment variables

```bash
cp .env.dist .env
```

Update `.env` with your Uptime Kuma server credentials and configuration.

### Environment Variables

| Variable | Required | Description | Example |
| --- | --- | --- | --- |
| `KUMA_SERVER` | Yes | Uptime Kuma instance URL | `https://uptime.example.com` |
| `KUMA_USERNAME` | Yes | Uptime Kuma username | `admin` |
| `KUMA_PASSWORD` | Yes | Uptime Kuma password | `password123` |
| `ADMIN_PASSWORD` | Yes | API admin password | `securepassword` |
| `ACCESS_TOKEN_EXPIRATION` | No | Token expiration in minutes | `11520` (8 days) |
| `SECRET_KEY` | No | JWT secret key | `your-secret-key` |
| `PORT` | No | Application port | `3001` |

### Start the application

```bash
python -m app.main
```

Then open the application at `http://localhost:3001`.

Access Swagger documentation at `http://localhost:3001/docs`.

## Deploy on Nife.io

You can deploy this application on [Nife.io](https://nife.io) using either the Git repository or the CLI.[1] [2]

### Option 1: Deploy from a Docker image


Configure a new application in Nife.io with the following settings.

| Setting | Value |
| --- | --- |
| Source | Docker Image |
| Registry | Docker Hub or another supported registry |
| Image | `louislam/uptime-kuma:1.19.6` |
| Internal Port | `3001` |
| External Port | `3001` |
| Environment Variables | key= `KUMA_SERVER` | value= `https://uptime.example.com` |
| Suggested Replicas | `1` |

### Option 2: Deploy from the Git repository

Deploy the project directly from GitHub.

| Setting | Value |
| --- | --- |
| Source | Git Repository |
| Provider | GitHub |
| Branch | `main` |
| Internal Port | `3001` |
| External Port | `3001` |
| Environment Variables | key = `KUMA_SERVER`   | value = `https://uptime.example.com` |
| Build Mode | Auto-Dockerize with runtime |

### Option 3: Deploy with `nifectl`

If you prefer the command line, use the following workflow.

```bash
nifectl auth login
nifectl init
nifectl deploy
```

For step-by-step instructions, see the [Nife.io Quick Deploy documentation](https://docs.nife.io/overview/quick-deploy) and the [nifectl quick start guide](https://docs.nife.io/Quick-Start/Nifectl).

## API Authentication

### Login to get access token

```bash
curl -X POST 'http://localhost:3001/login/access-token/' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  --data 'username=admin&password=<ADMIN_PASSWORD>'
```

### Use token for authenticated requests

```bash
TOKEN=$(curl -X POST 'http://localhost:3001/login/access-token/' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  --data 'username=admin&password=admin' | jq -r ".access_token")

curl -H 'Accept: application/json' \
  -H "Authorization: Bearer ${TOKEN}" \
  http://localhost:3001/monitors/
```

## Repository Structure

| Path | Purpose |
| --- | --- |
| `app/` | Main application code |
| `app/main.py` | Application entry point |
| `tests/` | Test files |
| `images/` | Documentation images |
| `requirements.txt` | Python dependencies |
| `Dockerfile` | Container build instructions |
| `docker-compose.yml` | Docker compose configuration |
| `.env.dist` | Environment variables template |
| `Makefile` | Build and deployment scripts |

## Troubleshooting

| Issue | Suggested fix |
| --- | --- |
| Python not installed | Install Python and verify with `python --version` |
| Dependencies missing | Run `pip install -r requirements.txt` |
| Uptime Kuma connection failed | Verify `KUMA_SERVER` URL and credentials |
| Admin password not set | Set `ADMIN_PASSWORD` environment variable |
| Port `8000` already in use | Stop the conflicting process or change the port |
| JWT token expired | Generate a new token with the login endpoint |
| Swagger docs not accessible | Ensure application is running at correct port |
| Deployment fails on Nife.io | Verify ports, environment variables, and build settings |
| Application is unreachable | Check routing, service exposure, and deployment logs |

## Acknowledgements

This repository is maintained by **Nifetency** as a sample deployment project for [Nife.io](https://nife.io).

This project is based on [Uptime-Kuma-API](https://github.com/lucasheld/uptime-kuma-api) and [Uptime Kuma](https://github.com/louislam/uptime-kuma). Proper credit should be given to the original authors.

## License

This project is licensed under the **MIT License**.

## References

1. [Nife.io](https://nife.io)
2. [Nife Docs Overview](https://docs.nife.io/overview/)
3. [Original Repository](https://github.com/nifetency/Uptime-Kuma-Web-API.git)
4. [Nife Quick Start](https://docs.nife.io/Quick-Start)
