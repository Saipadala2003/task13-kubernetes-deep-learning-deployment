# Task 13 Report Evidence

## Screenshots captured

1. Minikube status: control plane, kubelet and API server running.
2. Kubernetes node: `minikube Ready`, Kubernetes v1.37.0.
3. Minikube images: both Task 11 application images present.
4. `kubectl get all -n task13`: both Pods, Deployments and Services running.
5. `kubectl get pods -n task13 -o wide`: both Pods `1/1 Running`.
6. Flask health check: `model_loaded=true`, `status=healthy`.
7. Flask logs: Gunicorn listening on port 5000 and CNN model loaded successfully.
8. Streamlit browser: MNIST Digit Recognition application accessible.
9. Uploaded image: `7.png`.
10. Prediction result: digit 7 with 99.99% confidence.

## Result

The deep-learning application was successfully deployed and externally exposed on Kubernetes using Minikube. The Flask CNN API and Streamlit UI remained healthy, the internal service communication worked, and browser-based prediction was verified.
