# keycloak-postgresql-docker

This repository provides a simple Docker-Compose configuration to run a Keycloak instance with a PostgreSQL database. The setup supports both development and production modes.

## Table of Contents
1. [Architecture](#architecture)
2. [Prerequisites](#prerequisites)
3. [Configuration](#configuration)
4. [Starting the Environment](#starting-the-environment)
5. [Important Notes](#important-notes)

---

## Architecture

The environment includes:
- **Keycloak**: An open-source identity and access management solution.
  - Accessible via ports `7080` (HTTP) and `7443` (HTTPS). Change the values as necessary.
  - Uses a PostgreSQL database for persistence.
  - Supports operation in development or production mode. For production mode you have to configure certificates.
- **PostgreSQL**: A relational database to store Keycloak data.
  - Configured using environment variables for user credentials and database settings.
- **Bridge Network**: Enables communication between the containers.

---

## Prerequisites

Ensure the following are in place:
- Docker and Docker Compose installed on your system.
- Environment variables configured in .env-file:
  - `POSTGRES_USER`, `POSTGRES_PASSWORD`, `POSTGRES_DB` (PostgreSQL configuration).
  - `KEYCLOAK_ADMIN`, `KEYCLOAK_ADMIN_PASSWORD` (Keycloak admin credentials).
  - `KEYCLOAK_HOSTNAME` Hostname/static IP of your Docker host

---

## Configuration

1. **Verify the Docker-Compose File**:
   - Ensure that the file contains the correct service definitions and dependencies.
   - Check that the ports used do not conflict with existing services.

2. **Set Environment Variables**:
   - Configure provided `.env` file in the project directory:
     ```env
     POSTGRES_USER=keycloak
     POSTGRES_PASSWORD=securePassword
     POSTGRES_DB=keycloak

     KEYCLOAK_ADMIN=admin
     KEYCLOAK_ADMIN_PASSWORD=adminPassword
     KEYCLOAK_HOSTNAME=localhost
     ```
   - Adjust values to match your setup.

---

## Starting the Environment

1. **Start the Containers**:
   - Use the following command:
     ```bash
     docker-compose up --build
     ```

2. **Check Health Status**:
   - Keycloak:
     ```bash
     curl -f http://localhost:7080/health/ready
     ```
   - PostgreSQL:
     ```bash
     docker exec postgres pg_isready -U keycloak
     ```

3. **Access Keycloak**:
   - Development mode: [http://localhost:7080](http://localhost:7080)
   - Production mode: Configure HTTPS access as outlined in the documentation.

---

## Important Notes

- **Development Mode**:
  - Simplified setup without strict security requirements.
  - HTTP enabled, no distributed caching mechanisms.

- **Production Mode**:
  - HTTPS is mandatory.
  - For additional production configuration details, refer to [Keycloak Production Setup](https://www.keycloak.org/server/configuration-production).

---
