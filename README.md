# Smart IIoT ML Model Garbage Management System 🗑️🤖

[![ML-Model](https://img.shields.io/badge/ML-Random%20Forest-green)](https://scikit-learn.org/)
[![Hardware](https://img.shields.io/badge/Hardware-ESP32%20%7C%20HC--SR04%20%7C%20HX711-blue)](https://www.espressif.com/)
[![API](https://img.shields.io/badge/API-Flask-lightgrey)](https://flask.palletsprojects.com/)

An intelligent, IIoT-based waste monitoring solution that uses Machine Learning to predict bin status and optimize collection routes. This project moves beyond simple "if-else" logic by utilizing **Sensor Fusion** to prevent false alerts and ensure 100% reliability in waste detection.

## 🚀 Project Overview

Most smart bins rely solely on ultrasonic sensors, which often give false "Full" readings if an object is simply blocking the sensor. This system solves that by correlating:
1. **Volumetric Data** (Distance via Ultrasonic Sensor)
2. **Mass Data** (Weight via Load Cell)
3. **Temporal Context** (Hour of day and Weekend status)

The result is a **Random Forest Classifier** that determines if a bin is `Empty`, `Half-Full`, or `Overflow` with high precision.

---

## 🧠 Machine Learning Architecture

The "brain" of this system is a **Random Forest Ensemble** consisting of 100 Decision Trees. 

### Key Technical Features:
- **Sensor Fusion:** Merges Weight and Distance data to validate bin status.
- **Fuzzy Logic Implementation:** The model is trained on realistic, overlapping boundaries to handle sensor noise and edge cases.
- **Feature Scaling:** Uses `StandardScaler` to normalize disparate units (cm vs kg) for unbiased prediction.
- **Continuous Learning Loop:** Includes an automated retraining pipeline that updates the `.pkl` files as new sensor data is collected.



---

## 🛠️ Tech Stack

- **Languages:** Python 3.x, C++ (Arduino/ESP32)
- **Libraries:** Scikit-Learn, Pandas, Numpy, Joblib, Flask
- **Hardware:** ESP32/NodeMCU, HC-SR04 (Ultrasonic), HX711 (Load Cell)
- **Deployment:** REST API (Flask)

---

## 📂 File Structure

- `garbage_model.pkl`: The trained Random Forest model.
- `scaler.pkl`: The preprocessing scaler (Essential for normalization).
- `api_server.py`: Flask API to serve real-time predictions and trigger retraining.
- `real_world_garbage_data.csv`: The dataset containing historical sensor readings.
- `training_script.py`: The logic used to generate, train, and audit the model.

---

## 🔌 API Integration

The model is deployed as a microservice. You can interact with it via the following endpoints:

### 1. Get Prediction
**Endpoint:** `POST /predict`  
**Payload:**
```json
{
  "hour": 14,
  "weight": 42.5,
  "distance": 8.2,
  "weekend": 0
}
