```markdown
# User Profile API - README

## Overview

This project is a simple User Profile API built with Node.js, Express, and MongoDB.  
It supports CRUD operations on users and exposes REST endpoints.

The app runs inside Docker containers and is deployed on Kubernetes (minikube).  

---

## Prerequisites

- Docker installed and running  
- Kubernetes cluster or minikube installed and running  
- kubectl CLI configured for your cluster  
- MongoDB Atlas cluster or accessible MongoDB instance  
- Node.js and npm installed (for local testing)  

---

## Environment Variables

Set the following environment variable in your deployment or `.env` file:

- `MONGO_URI` - Your MongoDB connection string, e.g.:

```

mongodb+srv://<username>:<password>@cluster0.mongodb.net/mydatabase?retryWrites=true\&w=majority

````

---

## Build and Push Docker Image

1. Build the Docker image locally:

```bash
docker build -t your-dockerhub-username/profile-api:latest .
````

2. Push the image to Docker Hub (or your container registry):

```bash
docker push your-dockerhub-username/profile-api:latest
```

---

## Kubernetes Deployment

1. Update your `deployment.yaml` to use the pushed image:

```yaml
spec:
  containers:
  - name: profile-api
    image: your-dockerhub-username/profile-api:latest
    env:
      - name: MONGO_URI
        value: "<your-mongo-uri>"
    ports:
      - containerPort: 3000
```

2. Apply your deployment and service:

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

3. Check the pods status:

```bash
kubectl get pods
```

4. Check service details and port:

```bash
kubectl get svc
kubectl describe svc task9-service
```

---

## Accessing the API

If using minikube:

```bash
minikube service task9-service
```

This command opens the service in your default browser.
If the browser doesn't open, note the URL it outputs (e.g. `http://192.168.xx.xx:NodePort`), and access it directly.

---

## API Endpoints

| Method | Endpoint     | Description       |
| ------ | ------------ | ----------------- |
| POST   | `/users`     | Create a new user |
| GET    | `/users`     | Get all users     |
| GET    | `/users/:id` | Get user by ID    |
| PUT    | `/users/:id` | Update user by ID |
| DELETE | `/users/:id` | Delete user by ID |
| GET    | `/health`    | Health check      |

---

## Running Locally (Optional)

1. Create `.env` file with:

```
MONGO_URI=your-mongo-uri
PORT=3000
```

2. Install dependencies and run:

```bash
npm install
npm start
```

3. API runs on: `http://localhost:3000`

---

## Notes

* The server listens on `0.0.0.0` so it is accessible from outside the container.
* The service forwards port 80 to container port 3000 and uses a NodePort for external access.
* MongoDB connection URI must be valid and accessible from the Kubernetes cluster.
* Health endpoint is used for readiness and liveness probes.

---

## Troubleshooting

* If you get connection refused, check if pod is running and logs:

```bash
kubectl logs <pod-name>
```

* Verify the MongoDB URI and network access.
* Check Kubernetes service and NodePort.

---

## Example curl request

```bash
curl http://<minikube-ip>:<node-port>/users
```

##author #chandrakanth
---


