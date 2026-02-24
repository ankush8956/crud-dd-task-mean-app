In this DevOps task, you need to build and deploy a full-stack CRUD application using the MEAN stack (MongoDB, Express, Angular 15, and Node.js). The backend will be developed with Node.js and Express to provide REST APIs, connecting to a MongoDB database. The frontend will be an Angular application utilizing HTTPClient for communication.  

The application will manage a collection of tutorials, where each tutorial includes an ID, title, description, and published status. Users will be able to create, retrieve, update, and delete tutorials. Additionally, a search box will allow users to find tutorials by title.

## Project setup

### Node.js Server

cd backend

npm install

You can update the MongoDB credentials by modifying the `db.config.js` file located in `app/config/`.

Run `node server.js`

### Angular Client

cd frontend

npm install

Run `ng serve --port 8081`

You can modify the `src/app/services/tutorial.service.ts` file to adjust how the frontend interacts with the backend.

Navigate to `http://localhost:8081/`

## Docker & Deployment

This repository includes Dockerfiles for the backend and frontend, a local `docker-compose.yml`, and a production `docker-compose.prod.yml` that references images on Docker Hub.

High-level steps to deploy:

- Build and run locally (development):

```bash
This repository contains a full-stack CRUD example built with the MEAN stack (MongoDB, Express, Angular 15, Node.js).

The app manages a collection of tutorials (id, title, description, published). It supports creating, reading, updating, deleting, and searching tutorials by title.

**Quick production example**

- Frontend (SPA): http://13.126.150.47/
- Tutorials route: http://13.126.150.47/tutorials
- Public backend API (proxied): http://13.126.150.47/api/tutorials

## Project setup

### Backend (Node.js + Express)

1. Change to the backend folder:

```powershell
cd backend
```

2. Install dependencies and run locally:

```powershell
npm install
node server.js
```

3. Configuration:
- Edit `backend/app/config/db.config.js` to change defaults, or set `MONGODB_URI` to override.

Default fallback connection used by the app:

`mongodb://localhost:27017/dd_db`

Start with a custom URI:

```powershell
# Windows (PowerShell)
$env:MONGODB_URI='mongodb://user:pass@host:27017/your_db'; node server.js

# Linux/macOS
MONGODB_URI='mongodb://user:pass@host:27017/your_db' node server.js
```

### Frontend (Angular)

1. Change to the frontend folder and install:

```powershell
cd frontend
npm install
```

2. Run in development (Angular CLI):

```powershell
ng serve --port 8081
```

3. The Angular service that calls the API is `frontend/src/app/services/tutorial.service.ts`.

Open the app at `http://localhost:8081/` and navigate to the Tutorials view.

## Docker & Deployment

This repo includes Dockerfiles, `docker-compose.yml` (local) and `docker-compose.prod.yml` (production images).

### Local (development) with Docker

Build and run locally:

```bash
docker-compose up --build
```

### Production (VM) - high level

1. Create an Ubuntu VM and install Docker + Compose plugin:

```bash
sudo curl -fsSL https://get.docker.com -o get-docker.sh && sh get-docker.sh
sudo apt-get install -y docker-compose-plugin
```

2. Copy `docker-compose.prod.yml` to the VM (for example `~/docker-compose.prod.yml`).

3. CI/CD: GitHub Actions workflow can build images, push to Docker Hub, SSH to the VM, and run `docker-compose` to deploy. Configure secrets:

- `DOCKERHUB_USERNAME`
- `DOCKERHUB_TOKEN`
- `SSH_HOST`, `SSH_USER`, `SSH_PORT` (optional)
- `SSH_PRIVATE_KEY`

4. Example CI/VM commands run during deployment:

```bash
docker pull <yourhub>/angular-15-crud-backend:latest
docker pull <yourhub>/angular-15-crud-frontend:latest
docker-compose -f ~/docker-compose.prod.yml up -d
```

### Accessing the deployed app

- Frontend: `http://<VM_IP>/` (example: `http://13.126.150.47/`)
- Tutorials SPA route: `http://13.126.150.47/tutorials`
- Backend API: `http://13.126.150.47/api/tutorials`

Try the API:

```bash
curl http://13.126.150.47/api/tutorials
```

## Environment variables

- `MONGODB_URI` — MongoDB connection string (overrides `backend/app/config/db.config.js`).
- Backend default port: `8080`.
- Frontend (production) served by nginx on port `80` inside container.

## Troubleshooting

- If tutorials don't appear, confirm backend reachable at `/api/tutorials` and that MongoDB is healthy.
- View backend logs:

```bash
docker-compose logs backend
```

- View frontend/nginx logs:

```bash
docker-compose logs frontend
```

- Rebuild and restart production containers:

```bash
docker-compose -f ~/docker-compose.prod.yml pull
docker-compose -f ~/docker-compose.prod.yml up -d --remove-orphans
```

## Important files

- Backend: `backend/server.js`, `backend/app/controllers/`, `backend/app/models/`
- Frontend: `frontend/src/app/`, `frontend/nginx.conf`

---

If you want, I can also run the app locally and verify `http://13.126.150.47/api/tutorials` responses or update the CI workflow to your Docker Hub/VM settings.

