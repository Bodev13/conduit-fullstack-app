# Conduit Fullstack App


This is a fullstack Conduit application built with a Python backend and a Angular frontend.  
The project is containerized using Docker and orchestrated with Docker Compose.


### Prerequisites

- Docker (with Docker Compose plugin) installed


## Table of Contents

- [Quickstart](#quickstart)
- [Usage](#usage)
- [Logs](#logs)
- [Rebuilding containers](#rebuilding-containers)
- [CI/CD Deployment](#ci/cd-deployment)



## Quickstart

1. Clone the repository with its BE and FE submodules

```bash
git clone --recurse-submodules git@github.com:Bodev13/conduit-fullstack-app.git
cd conduit-fullstack-app
```

2. Update and initialize the submodules

```bash
git submodule update --init --recursive
```

3. Create and set up `.env` file by following the description below in the section [Environments](#environments)

4. Copy the `template.env` to your `.env` and add your cloud VM IP address

```bash
cp template.env .env
```

5. Build and start the Conduit Fullstack App container

```bash
docker compose up --build
```
6. Access the Conduit app locally

• FE
```bash
http://localhost:8282
```

• BE
```bash
http://localhost:8000/admin
```


## Usage

This will get the conduit app running in a Docker container on your local machine and on the V-Server. The build and run process is managed by the docker-compose.yml file.

It contains two services:

• conduit-backend service: builds and starts the conduit backend application

• conduit-frontend service: builds and starts the conduit frontend application


### Environments

• Set up `.env` file and make sure it is added to the [.gitignore](./.gitignore). Just copy the `template.env` file to create your own `.env` by the following command

```bash
cp template.env .env
```

| Name                          | Type   | Description                                                        |
|-------------------------------|--------|--------------------------------------------------------------------|
| `API_URL`                     | string | Backend API server URL                                             |
| `ALLOWED_HOSTS`               | list   | List of host/domain names this Django site can serve              |
| `SECRET_KEY`                  | string | Django secret key for cryptographic operations                     |
| `DEBUG`                       | bool   | Enables debug mode (only use in development)                       |
| `DJANGO_SUPERUSER_USERNAME`  | string | Superuser name used during initial migration                       |
| `DJANGO_SUPERUSER_EMAIL`     | string | Email address of the Django superuser                              |
| `DJANGO_SUPERUSER_PASSWORD`  | string | Password for the Django superuser                                  |
| `FRONTEND_URL`               | list   | List of frontend URLs allowed to communicate with the backend      |
| `FRONTEND_PORT`              | number | Port where the frontend is running                                 |

• Create secret keys (for `.env`)

```bash
from django.core.management.utils import get_random_secret_key
print(get_random_secret_key())
```
• Run the app

```bash
docker compose up --build
```

• Interact with your app

• FE
```bash
API: http://your_server_ip:8282
```
• BE
```bash
API: http://your_server_ip:8000/admin
```

## Logs
You can save logs using

BE logs
```bash
docker compose logs backend > backend-logs.txt
```

FE logs
```bash
docker compose logs frontend > frontend-logs.txt
```

All logs
```bash
docker compose logs > all-logs.txt
```

## Rebuilding containers
You can turn off and rebuild the container with the following commands

Bare in mind that `-v` by the command below will delete all volumes bound in the `docker-compose`, which is not the best practice in production environment. For production you can use the command without `-v`

```bash
docker compose down -v
docker compose up --build
```

## CI/CD Deployment

This project uses GitHub Actions to automate the deployment of the Conduit Fullstack App to a Cloud VM. The CI/CD workflow runs automatically whenever you push to the deployment branch (`cicd-actions-build` or `main`) and handles the following steps

- Checkout repository and submodules

The workflow clones the repository along with the backend and frontend submodules

- Build Docker images on GitHub Actions

• Backend and frontend services are built into Docker images
• Environment variables from `.env` are passed as build arguments
• Each image is tagged with:

  • `latest` → convenient for staging or quick updates
  • `${{ github.sha }}` → immutable, tied to the specific commit (recommended for production)

- Push images to GitHub Container Registry (GHCR)

The workflow pushes the images to the GHCR so they can be pulled by your Cloud VM

- Deploy to Cloud VM via SSH

• Connects to the VM using a secure SSH key from GitHub Secrets
• It navigates to the project directory on the VM and prepares the environment
• `set -e` ensures that if any command fails, the workflow stops automatically
`--remove-orphans` deletes containers that are no longer defined in your docker-compose.yml
`docker image prune -f` cleans up unused images to prevent disk clutter

- Required GitHub Secrets

To make the deployment work, the following secrets must be created in your repository settings

| Name              | Type   | Description                                                     |
|-------------------|--------|-----------------------------------------------------------------|
| `SERVER_IP`       | string | Public IP address of the Cloud VM                               |
| `SERVER_USER`     | string | SSH username for accessing the Cloud VM                         |
| `SERVER_SSH_KEY`  | string | Private SSH key used by GitHub Actions to connect to the VM     |
| `GITHUB_TOKEN`    | string | Default GitHub Actions token used to authenticate with GHCR    |

### How to trigger deployment

Deployment happens automatically when you push changes to the branch `cicd-actions-build`

You can also trigger the workflow manually from the `Actions tab` in GitHub.