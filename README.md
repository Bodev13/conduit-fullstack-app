# Conduit Fullstack App


This is a fullstack Conduit application built with a Python backend and a Angular frontend.  
The project is containerized using Docker and orchestrated with Docker Compose.


### Prerequisites

- Docker & Docker Compose installed
- Setup the FE and BE apps separately
BE: [conduit-backend](https://github.com/Bodev13/conduit-backend)
FE: [conduit-frontend](https://github.com/Bodev13/conduit-frontend)


## Table of Contents

- [Quickstart](#quickstart)
- [Usage](#usage)
- [Logs](#logs)
- [Rebuilding containers](#rebuilding-containers)



## Quickstart

1. Clone the repository

```bash
git@github.com:Bodev13/conduit-fullstack-app.git
cd conduit-fullstack-app
````
2. Create and set up .env file
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
http://your_cloud_vm:8282
```
6. Stop and delete containers
```bash
docker-compose down
```

## Usage

This will get the conduit app running in a Docker container on your local machine and on the V-Server. The build and run process is managed by the docker-compose.yml file.

It contains two services and a volume:

• conduit-backend service: builds and starts the conduit backend application
• conduit-frontend service: builds and starts the conduit frontend application
• db volume called "backend_data": bound to the conduit backend application's database

### Environments

• Set up .env file and make sure it is added to the .gitignore. Just copy the template.env file to create your own .env by the following command

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

   • SECRET_KEY
```bash
from django.core.management.utils import get_random_secret_key
print(get_random_secret_key())
```
• Run the app

```bash
docker-compose up --build
```

• Interact with the app

```bash
API: http://your_server_ip:8000/admin
```

## Logs
You can save logs using

```bash
docker logs backend > backend-logs.txt
```

## Rebuilding containers
You can turn off and rebuild the container with the following commands

```bash
docker-compose down
docker-compose up --build
```