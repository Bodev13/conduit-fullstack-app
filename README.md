# Conduit Fullstack App


This is a fullstack Conduit application built with a Python backend and a Angular frontend.  
The project is containerized using Docker and orchestrated with Docker Compose.


### Prerequisites

- Docker & Docker Compose installed


## Table of Contents

- [Quickstart](#quickstart)
- [Usage](#usage)
- [Logs](#logs)
- [Rebuilding containers](#rebuilding-containers)



## Quickstart

1. Clone the repository with its BE and FE submodules

```bash
git clone --recurse-submodules git@github.com:Bodev13/conduit-fullstack-app.git
cd conduit-fullstack-app
````
2. Create and set up .env file by following the description below in the section [Environments](#environments)

3. Build and start the Conduit Fullstack App container

```bash
docker-compose up --build
```
4. Access the Conduit locally

• The frontend at
```bash
http://localhost:8282
```

• The backend at
```bash
http://localhost:8000/admin
```

5. On your cloud VM at
```bash
http://your_cloud_vm_ip:8282
```
6. Stop and delete containers
```bash
docker-compose down
```

## Usage

This will get the conduit app running in a Docker container on your local machine and on the V-Server. The build and run process is managed by the docker-compose.yml file.

It contains two services:

• conduit-backend service: builds and starts the conduit backend application

• conduit-frontend service: builds and starts the conduit frontend application


### Environments

• Set up `.env` file and make sure it is added to the [.gitignore](./.gitignore). Just copy the template.env file to create your own .env by the following command

```bash
cp template.env .env
```

| Name                          | Type   | Description                                                        |
|-------------------------------|--------|--------------------------------------------------------------------|
| `API_URL`                     | string | Backend API server URL                                             |
| `ALLOWED_HOSTS`               | list   | List of domains/IPs allowed to access the application              |
| `SECRET_KEY`                  | string | Django secret key for cryptographic operations                     |
| `DEBUG`                       | bool   | Enables debug mode (only use in development)                       |
| `DJANGO_SUPERUSER_USERNAME`  | string | Superuser name used during initial migration                       |
| `DJANGO_SUPERUSER_EMAIL`     | string | Email address of the Django superuser                              |
| `DJANGO_SUPERUSER_PASSWORD`  | string | Password for the Django superuser                                  |
| `FRONTEND_URL`               | list   | List of frontend URLs allowed to communicate with the backend      |
| `FRONTEND_PORT`              | number | Port where the frontend is running                                 |

• Create secret keys (for .env file)

```bash
from django.core.management.utils import get_random_secret_key
print(get_random_secret_key())
```
• Run the app

```bash
docker-compose up --build
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

```bash
docker-compose logs backend > backend-logs.txt
```

## Rebuilding containers
You can turn off and rebuild the container with the following commands

```bash
docker-compose down
docker-compose up --build
```