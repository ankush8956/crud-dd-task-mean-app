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
docker-compose up --build
```

- Production (VM):
	- Create an Ubuntu VM and install Docker and Docker Compose.
	- Copy `docker-compose.prod.yml` to the VM (e.g. `~/docker-compose.prod.yml`).
	- Ensure the VM has Docker and docker-compose installed and that port 80 is open.

- CI/CD: the provided GitHub Actions workflow builds and pushes images to Docker Hub and SSHes into your VM to pull and restart containers. Configure these repository secrets:
	- `DOCKERHUB_USERNAME` and `DOCKERHUB_TOKEN` (or password)
	- `SSH_HOST`, `SSH_USER`, `SSH_PORT` (optional; default 22), and `SSH_PRIVATE_KEY`

After pushing to `main`, the workflow will build images and run on the VM:

```bash
# on VM (one-time setup)
sudo curl -fsSL https://get.docker.com -o get-docker.sh && sh get-docker.sh
sudo apt-get install -y docker-compose-plugin

# place docker-compose.prod.yml at ~/docker-compose.prod.yml

# workflow will run:
docker pull <yourhub>/angular-15-crud-backend:latest
docker pull <yourhub>/angular-15-crud-frontend:latest
docker-compose -f ~/docker-compose.prod.yml up -d
```

Notes:
- The backend reads MongoDB connection from `MONGODB_URI` environment variable (falls back to `mongodb://localhost:27017/dd_db`).
- Nginx inside the frontend container serves the Angular app and proxies API requests under `/api/` to the backend (`backend:8080`).

