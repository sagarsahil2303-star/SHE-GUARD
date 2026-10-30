# SHE-GUARD
### AI-Powered IoT Personal Safety and Intelligent Emergency Response System

**SHE-GUARD** is an intelligent personal safety system that combines Artificial Intelligence, Machine Learning, IoT sensors, audio distress detection, motion analysis and location-based risk intelligence to improve personal safety through proactive threat detection and emergency response.

The project aims to move beyond conventional SOS-based safety applications by identifying potential distress situations automatically and initiating an emergency response workflow.

---

## 1. Problem Statement

Existing personal safety solutions largely depend on users manually triggering an SOS alert. However, during threatening situations, individuals may not always be able to access or operate their smartphones.

SHE-GUARD addresses this limitation through an intelligent wearable system capable of monitoring distress-related audio and motion patterns, along with location-based environmental risk information.

## 2. Proposed Solution

SHE-GUARD integrates three intelligent modules:

- **Audio Distress Detection:** Identifies potential scream and distress audio patterns using a trained Machine Learning model.
- **Motion Analysis:** Analyzes wearable sensor data to classify physical activities and support future abnormal-motion detection.
- **Risk Intelligence:** Uses geographical crime statistics and risk-scoring logic to identify potentially unsafe areas and recommend safer alternatives.

These modules work with an IoT-enabled wearable and a web-based monitoring interface to provide an integrated personal safety solution.

## 3. System Architecture

The system follows a modular architecture:

1. Wearable hardware collects sensor information.
2. ESP32-S3 processes and transmits sensor data through Wi-Fi.
3. Backend APIs receive and process incoming information.
4. Machine Learning modules analyze audio and motion inputs.
5. Risk intelligence evaluates geographical safety information.
6. The frontend displays system status, risk information and alerts.
7. An emergency countdown workflow allows the user to cancel a false alert before activation.

### Overall Architecture

```text
             USER / WEARABLE
                    |
              ESP32-S3
                    |
          Wi-Fi Communication
                    |
             BACKEND API
                    |
       +------------+------------+
       |            |            |
   AUDIO ML      MOTION ML    RISK ENGINE
       |            |            |
   Distress      Activity     Location
   Detection    Classification Risk Score
       |            |            |
       +------------+------------+
                    |
             ALERT ENGINE
                    |
          10-SECOND COUNTDOWN
                    |
             WEB DASHBOARD
                    |
         EMERGENCY RESPONSE FLOW
```

## 4. Hardware Architecture

| Component | Purpose |
|---|---|
| ESP32-S3 N16R8 | Main IoT microcontroller |
| Microphone module | Audio acquisition |
| Motion sensor / IMU | Motion data acquisition |
| Power supply | Portable wearable operation |
| Wearable enclosure | Physical integration |

**Microcontroller:** ESP32-S3 N16R8.

The wearable is designed to transmit sensor information to the backend using wireless communication.

Hardware integration is under development and is being validated alongside the software prototype.

## 5. Machine Learning Architecture

### Model 1: Audio Distress Detection

**Objective:** Detect potential scream and distress audio events.

**Dataset and preprocessing:**
- Audio samples are processed at 22,050 Hz.
- Audio is divided into 0.5-second windows.
- Sliding-window interval: 0.25 seconds.
- MFCC feature extraction is performed using 13 coefficients.
- Mean and standard deviation statistics are extracted from the audio features.

**Model:**
- Random Forest Classifier.
- 300 decision trees.
- Classes: `not_scream` and `scream`.
- Detection threshold: 0.6.

**Model artifact:** `she_guard_short_window_candidate.zip`

The artifact includes the trained model, configuration and inference script.

### Model 2: Motion Analysis

**Objective:** Analyze wearable motion sensor information.

**Dataset:** PAMAP2 Physical Activity Monitoring Dataset.

**Input features:**
- Wrist accelerometer measurements.
- Wrist gyroscope measurements.
- Six sensor channels.

**Preprocessing:**
- Sensor data segmentation into fixed windows.
- Window length: 200 samples.
- Feature extraction: 24 features.
- Random Forest classification.

**Training results:**
- Total processed windows: 521.
- Walking windows: 290.
- Running windows: 231.
- Training windows: 304.
- Testing windows: 217.
- Reported test accuracy: 94.93%.
- Mean accuracy across three held-out subjects: 96.09%.

**Current limitation:** The trained classifier differentiates walking and running activities. It is not yet a validated distress or anomaly detector.

Future development will focus on abnormal movement detection and distress-related motion patterns.

### Model 3: Geographical Risk Intelligence

**Objective:** Estimate potential geographical safety risks using crime statistics.

**Dataset:** Karnataka crime statistics covering 2023–2025.

**Dataset characteristics:**
- 124 records.
- District or unit-level information.
- IPC/BNS reported cases.
- SLL reported cases.
- Total reported cases.
- Geographic and source information.

**Processing and implementation:**
- Crime data collection and preprocessing.
- Geographical risk estimation.
- Risk-scoring logic.
- Location-based safety information.
- Safer-route recommendation concept.

**Important limitation:** The current dataset does not provide sufficient validated zone-level and time-based information for genuine zone-level ML training. The current demonstration uses simulated risk scoring and should not be interpreted as a validated crime prediction model.

## 6. Software Architecture

| Layer | Technology |
|---|---|
| Microcontroller | ESP32-S3 N16R8 |
| Programming | Python, C/C++ |
| ML development | Google Colab |
| ML framework | Scikit-learn |
| Audio processing | Librosa |
| Data processing | Pandas, NumPy |
| Backend | Python, FastAPI |
| Frontend | Emergent |
| Communication | Wi-Fi, HTTP APIs |
| Data visualization | Web dashboard |

## 7. Backend API Architecture

The backend provides endpoints for communication between the wearable, machine learning modules and frontend.

| Endpoint | Function |
|---|---|
| `GET /health` | Backend health check |
| `GET /sensors/test` | Sensor communication testing |
| `POST /sensors/data` | Receive sensor data |
| `POST /sensors/simulate` | Simulate sensor inputs |
| `POST /audio/analyze` | Audio distress analysis |
| `POST /motion/analyze` | Motion analysis |
| `POST /risk/estimate` | Geographical risk estimation |
| `GET /alerts` | Retrieve alert information |

The backend uses modular processing to keep sensor acquisition, model inference and alert management logically separated.

## 8. Emergency Alert Workflow

SHE-GUARD introduces an automated emergency alert workflow.

**Detection → Verification Window → Emergency Alert**

1. Potential distress audio is detected.
2. The backend generates a pending alert.
3. A 10-second countdown is initiated.
4. The user can cancel the alert if it is a false positive.
5. If not cancelled, the alert transitions to active status.
6. Emergency response information is presented through the application.

Duplicate alerts are restricted through a 60-second suppression window.

**Communication status:** The current SMS and voice interfaces are demonstration features. The demo SMS is simulated and browser text-to-speech does not place an actual telephone call. Real SMS and telephony integration require an external communication provider.

## 9. Frontend and User Interface

The SHE-GUARD web application is developed using Emergent.

The interface is designed to provide:

- System health monitoring.
- Audio distress detection status.
- Motion analysis information.
- Geographical risk information.
- Emergency alert monitoring.
- Countdown and cancellation interface.
- Emergency contact demonstration interface.

The frontend and backend prototype have been developed in Emergent.

## 10. Repository Structure

```text
SHE-GUARD/
│
├── README.md
│
├── datasets/
│   ├── audio_dataset/
│   ├── motion_dataset/
│   └── crime_dataset/
│
├── models/
│   ├── audio_model/
│   │   ├── model.zip
│   │   ├── audio_predictor_v2.py
│   │   └── short_window_config.json
│   │
│   ├── motion_model/
│   │   └── trained_model_files
│   │
│   └── risk_model/
│       └── risk_scoring_files
│
├── notebooks/
│   ├── SHE_GUARD_Audio_Model.ipynb
│   ├── SHE_GUARD_Motion_ML_v1.ipynb
│   └── GEO_PREDICTION_SHE_GUARD.ipynb
│
├── documentation/
│   ├── system_architecture.png
│   └── model_architecture.png
│
├── requirements.txt
│
└── LICENSE
```

*The folder structure represents the intended organization. Include only files that have actually been created.*

## 11. Model Training and Reproducibility

All three model development workflows are documented through Google Colab notebooks.

The notebooks provide the respective data preparation, feature extraction, model training, evaluation and experimentation workflows.

To reproduce the experiments:

1. Open the relevant `.ipynb` notebook.
2. Open it in Google Colab.
3. Obtain the corresponding dataset from its original source.
4. Configure the dataset path.
5. Install the required dependencies.
6. Execute the notebook cells sequentially.
7. Evaluate the resulting model.

Dataset licensing and original dataset attribution must be respected.

## 12. Testing and Validation

The software prototype has been tested through backend API endpoints and frontend demonstration workflows.

Validation areas include:

- Backend health checks.
- Sensor data submission.
- Audio model inference.
- Motion model inference.
- Risk estimation.
- Alert creation.
- Countdown and cancellation.
- Frontend-backend communication.

Model performance figures represent experimental evaluation results and are not equivalent to real-world safety validation.

## 13. Current Development Status

| Module | Status |
|---|---|
| Audio ML model | Trained |
| Motion ML classifier | Trained |
| Risk intelligence | Demo scoring |
| Backend API | Developed and tested |
| Frontend dashboard | Developed |
| Alert countdown | Implemented |
| Real SMS integration | Not implemented |
| Real voice calling | Not implemented |
| Hardware integration | Partial |
| Motion distress anomaly detection | Future development |

## 14. Future Scope

- Advanced abnormal-motion and fall detection.
- Improved scream and distress recognition.
- Real-time wearable sensor integration.
- GPS-based location intelligence.
- Validated zone-level safety analysis.
- Integration with emergency communication providers.
- Battery optimization and wearable miniaturization.
- Real-world testing and model improvement.
- Privacy-focused data handling and security.

## 15. Project Vision

SHE-GUARD aims to develop an intelligent, proactive personal safety ecosystem that combines wearable technology, machine learning and geographical intelligence.

Rather than relying exclusively on manual emergency activation, the system explores how technology can help recognize potential distress situations and support timely emergency response.

**SHE-GUARD — Intelligent Safety. Proactive Protection.**

---

## Disclaimer

SHE-GUARD is an academic and prototype-stage project developed for research, experimentation and demonstration purposes.

It is not a certified emergency response system. Its predictions and alerts may be inaccurate, and it must not replace established emergency services or personal safety measures.

## License

This project is intended to be released under the MIT License, subject to the licensing conditions of all third-party datasets, models and dependencies included in the repository.

## Contributors

**SHE-GUARD Development Team**

- Sahil Sagar
- S. Sahana
- Imran B.
- Smaran Jain

**Institution:** PES College of Engineering, Mandya  
**Department:** Electronics and Communication Engineering

---

*Built with the vision of making personal safety more intelligent, accessible and proactive.*
