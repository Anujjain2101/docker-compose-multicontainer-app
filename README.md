# Docker Compose - Multi-Container Application

## Project Overview

A Flask-based web application orchestrated using Docker Compose.

The application uses:
- **Flask** – Web application
- **PostgreSQL** – Persistent relational database
- **Redis** – In-memory counter/cache

Docker Compose manages the application services, networking, environment variables, health checks, restart policies, and persistent storage.

## Architecture

```text
                 Host Machine
                      |
                localhost:5000
                      |
                Docker Compose
                      |
                 Web Service
                 Flask App
                  /     \
                 /       \
          Redis Service   PostgreSQL Service
          Port 6379       Port 5432
          Cache/Counter  Persistent Database
                              |
                         Named Volume
                            pgdata


**Understand the architecture:**

- Flask connects to PostgreSQL using the hostname `db`.
- Flask connects to Redis using the hostname `redis`.
- These hostnames resolve through Docker Compose's internal network.
- PostgreSQL data is stored in the named volume `pgdata`.
- Only Flask's port `5000` is published to the host. PostgreSQL and Redis are not exposed to the host.

**Interview question: Why does Flask connect to `db:5432` instead of `localhost:5432`?**

Because `localhost` inside a container refers to that same container. Compose provides internal DNS, allowing services to communicate using their service names.

Save the README and tell me when done. We'll document setup and execution commands next.

## Prerequisites

- Docker Desktop
- Docker Compose v2
- Git

Verify installations:

```bash
docker --version
docker compose version
git --version

## Application Testing

### 1. Test Flask Application

```bash
curl http://localhost:5000/

# View all containers, including stopped containers
docker compose ps -a

# View service logs
docker compose logs

# Follow application logs
docker compose logs -f web

# Inspect container configuration
docker inspect day12-compose-web-1

# Check resource usage
docker stats

# Validate Compose configuration
docker compose config

# List Docker volumes
docker volume ls

# List Docker networks
docker network ls