# 🚧 PotholeShack: Real-Time Pothole Detection & Safe Navigation

PotholeShack is an autonomous, AI-driven road hazard detection and navigation system. Using a vehicle-mounted camera running YOLOv8, it detects potholes in real-time, maps them via GPS, and syncs the data to the cloud. The accompanying web dashboard provides drivers with "Safe Routing" algorithms that calculate the best path to avoid vehicular damage, complete with voice alerts for approaching hazards.

---

## ✨ Features

- **Live Edge-AI Detection**: Processes live video feeds using Ultralytics YOLOv8 to identify potholes and speed breakers.
- **Smart Data Batching**: Tracks objects and intelligently batches detections over distance intervals (e.g., every 50 meters) to save bandwidth and prevent database hammering.
- **"Safest" Routing Algorithm**: Evaluates Google Maps alternative routes and scores them based on the density and severity of potholes along the path.
- **Real-Time Heatmap & Dashboard**: A web-based UI built with Leaflet and Google Maps API to visualize road hazards globally.
- **Proactive Voice Alerts**: Warns drivers via audio when approaching a detected pothole.
- **Live Telemetry Dashboard**: Displays current speed, ETA, and estimated fuel/maintenance savings based on avoided potholes.

---

## 🛠 Tech Stack

- **Computer Vision Model**: YOLOv8 (Ultralytics), PyTorch
- **Video & Image Processing**: OpenCV
- **Backend & Database**: Firebase Firestore (NoSQL), Firebase Admin SDK
- **Frontend / Dashboard**: HTML5, CSS3, Bootstrap 5, JavaScript
- **Mapping & Routing**: Google Maps Directions API, Leaflet, Folium

---

## 🚀 Getting Started

### Prerequisites

1. **Python 3.8+**
2. **Firebase Account**: Set up a Firebase project and enable Firestore. Download the Admin SDK private key.
3. **Google Maps API Key**: Requires Maps JavaScript API and Directions API enabled.

### Installation

1. Clone the repository and navigate to the project directory.
2. Install the required Python dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. **Firebase Configuration**:
   - Place your Firebase Admin SDK JSON file in the root directory and rename it to match the path in `detect.py` (e.g., `potholeshack-6d5f9-firebase-adminsdk-fbsvc-1315467f66.json`).
   - Update `firebase-config.js` with your Firebase web configuration.
4. **Google Maps Configuration**:
   - Add your Google Maps API key to `map.html`.

---

## 💻 Usage

### 1. Training the Model (Optional)
If you wish to train your own model on custom data:
```bash
python train.py
```
*Note: Ensure you have your Roboflow API key configured in `data.py` to download the dataset.*

### 2. Running Live Detection
Mount your camera to your vehicle's dashboard and run the live edge detection script. The script will automatically poll GPS coordinates and upload detections.
```bash
python detect.py
```

### 3. Using the Navigation Dashboard
Simply open the navigation UI in any modern web browser:
```bash
start map.html  # On Windows
```
Enter your starting point and destination, select your preferred routing mode (Safest, Fastest, Balanced), and start your journey!

### 4. Generating Mock Data
To test the routing algorithms without driving, you can populate your Firestore database with simulated data:
```bash
python generate_dummy_logs.py
```

---

## 🏗 Architecture Workflow

1. **Edge Inference**: `detect.py` captures video frames and runs YOLOv8.
2. **GPS Synchronization**: A parallel background thread fetches coordinates seamlessly.
3. **Data Aggregation**: Detections are evaluated for severity, grouped logically, and pushed to Firebase Firestore.
4. **Client Consumption**: `map.html` establishes a live connection to Firestore, overlaying hazards onto the map.
5. **Route Evaluation**: When a user requests directions, the client evaluates all potential routes against the spatial pothole data to recommend the safest path.

---

## ⚠️ Security Notice

This codebase is provided as a **prototype demonstration**. 
- It currently stores Firebase Admin SDK credentials locally. 
- In a production environment, all direct database writes should be migrated to a secure backend API (e.g., Firebase Cloud Functions or a Python FastAPI proxy).
- API keys in the frontend must be restricted via HTTP referrers in your Google Cloud / Firebase consoles.

---

## 🛣 Roadmap

- [ ] Transition from IP-based geolocation to NMEA serial parsing for physical GPS modules.
- [ ] Implement server-side spatial database queries (e.g., PostgreSQL + PostGIS) to handle millions of data points without crashing the client browser.
- [ ] Containerize the edge detection pipeline using Docker.
- [ ] Add a data-decay mechanism to automatically remove patched potholes after prolonged periods of no detection.
