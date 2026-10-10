# Task 13: Deploying Deep Learning Applications on Kubernetes

## Objective
Deploy and expose the containerized MNIST deep-learning application in a Kubernetes environment using Minikube.

## Application
This project uses the Task 11 MNIST application:
- Flask + TensorFlow CNN prediction API
- Streamlit frontend
- Docker images:
  - `task11-flask-api:latest`
  - `task11-streamlit-ui:latest`

## Kubernetes architecture

```text
                 Browser
                    |
                    v
        Streamlit UI - NodePort :30081
                    |
                    | HTTP
                    v
        Flask CNN API - ClusterIP :5000
                    |
                    v
              CNN Model
          deep_learning_model.h5
```

## Kubernetes resources

| Resource | Name | Purpose |
|---|---|---|
| Namespace | task13 | Isolates Task 13 resources |
| Deployment | flask-api | Runs the CNN prediction API |
| Service | flask-api | Internal ClusterIP access on port 5000 |
| Deployment | streamlit-ui | Runs the Streamlit frontend |
| Service | streamlit-ui | External NodePort access on port 30081 |

## Prerequisites
- Docker Desktop
- Minikube
- kubectl
- Task 11 Docker images available locally

## Deployment

Run from the `k8s` directory:

```powershell
minikube start --driver=docker
minikube image load task11-flask-api:latest
minikube image load task11-streamlit-ui:latest

kubectl apply -f namespace.yaml
kubectl apply -f flask-deployment.yaml
kubectl apply -f flask-service.yaml
kubectl apply -f streamlit-deployment.yaml
kubectl apply -f streamlit-service.yaml
```

## Validation

```powershell
minikube status
kubectl get nodes
kubectl get all -n task13
kubectl get pods -n task13 -o wide
kubectl get services -n task13
```

Verify the model-loaded health endpoint through the Kubernetes network:

```powershell
kubectl exec -n task13 deployment/streamlit-ui -- python -c "import urllib.request; print(urllib.request.urlopen('http://flask-api:5000/health').read().decode())"
```

Expected response:

```json
{"model_loaded":true,"status":"healthy","success":true}
```

Open the external application:

```powershell
minikube service streamlit-ui -n task13
```

Upload an MNIST digit image and click **Predict Digit**.

## Verified result

The submitted execution evidence shows:
- Minikube control plane, kubelet and API server running.
- `minikube` node in `Ready` state.
- Both Task 11 Docker images loaded into Minikube.
- Flask and Streamlit Pods running with `1/1` readiness.
- Flask ClusterIP service on TCP 5000.
- Streamlit NodePort service on TCP 8501 exposed as 30081.
- Flask health endpoint reports `model_loaded=true` and `status=healthy`.
- Flask logs confirm the CNN model loaded from `/app/deep_learning_model.h5`.
- Streamlit successfully predicts digit **7** with **99.99% confidence**.

## Report
The repository is intended for LMS trainer review together with the Task 13 PDF report containing source code, screenshots, outputs, results and observations.
