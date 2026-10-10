# Task 13 Architecture

## Request flow

1. User opens the Streamlit application exposed by the NodePort Service.
2. Streamlit receives an uploaded MNIST digit image.
3. Streamlit sends the image to the internal Flask Service at `http://flask-api:5000/predict`.
4. Flask preprocesses the image to 28x28 grayscale.
5. The TensorFlow/Keras CNN model predicts the digit.
6. Flask returns the predicted digit, confidence, and class probabilities.
7. Streamlit displays the prediction and confidence to the user.

## Kubernetes design

- **Namespace:** task13
- **Flask Deployment:** one Pod, health probes, local image
- **Flask Service:** ClusterIP, internal-only access
- **Streamlit Deployment:** one Pod, health probes, environment variable for API URL
- **Streamlit Service:** NodePort 30081 for external browser access
