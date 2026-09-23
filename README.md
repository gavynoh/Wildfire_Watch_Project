# Early Wildfire Detection

A computer-vision prototype for detecting wildfire presence in satellite imagery and recording reported wildfire locations through an interactive web application.

A personal introductory exploration of machine learning and its potential applications in environmental monitoring.

## Overview

Wildfires can be difficult to detect quickly across large and remote areas. This project explores whether a convolutional neural network (CNN) can assist with the classification of satellite imagery for wildfire presence.

The project combines a trained **TensorFlow/Keras** image-classification model with a **Flask** web application. Users can upload an image for analysis, receive a wildfire/no-wildfire prediction with a confidence score, and, following a positive detection, manually enter coordinates for display on an interactive map.

The original project was developed using a dataset of **44,000+ satellite images** containing forested areas with and without wildfires.

## Application Workflow

1. A user uploads a PNG or JPEG satellite image.
2. OpenCV reads and resizes the image to **32 × 32 pixels**.
3. The image is converted from BGR to RGB and pixel values are normalized.
4. The processed image is passed to the TensorFlow/Keras CNN.
5. The application returns a wildfire/no-wildfire prediction with a confidence score.
6. If a wildfire is detected, the user can enter its latitude and longitude.
7. Folium records and displays the reported wildfire coordinates on an interactive map.

## Model Architecture

The deployed model is a sequential convolutional neural network implemented with **TensorFlow/Keras**.

Its architecture includes:

- 2D convolutional layer with 32 filters and ReLU activation
- 2D convolutional layer with 64 filters and ReLU activation
- Max pooling
- Batch normalization
- Flattening
- 20% dropout
- 64-unit fully connected layer with ReLU activation
- Two-class softmax output layer

The model accepts RGB images with an input shape of:

```text
32 × 32 × 3
```

The repository contains the deployed model architecture and its trained model weights.

## Image Preprocessing

Before inference, uploaded images are processed using OpenCV:

- Read from the uploaded file
- Resized to `32 × 32`
- Converted from BGR to RGB
- Normalized from pixel values of `0–255` to `0–1`
- Reshaped into a batch with dimensions `1 × 32 × 32 × 3`

The processed image is then passed to the CNN for classification.

## Technologies

### Machine Learning
- Python
- TensorFlow
- Keras
- Convolutional Neural Networks
- Computer Vision

### Image Processing
- OpenCV
- NumPy

### Web Application
- Flask
- HTML
- JavaScript
- Bootstrap
- Werkzeug

### Mapping and Deployment
- Folium
- Gunicorn
- Render

## Repository Structure

```text
.
├── app.py                 # Flask backend and prediction endpoints
├── model_file.py          # TensorFlow/Keras CNN architecture
├── wildfire.weights.h5    # Trained model weights
├── templates/
│   └── index.html         # Web interface and client-side JavaScript
├── requirements.txt       # Python dependencies
├── render.yaml            # Render deployment configuration
├── runtime.txt            # Python runtime specification
├── .gitignore
└── README.md
```

## Web Application

The application uses Flask to connect the trained CNN to a browser-based interface.

### Image Classification

Users can upload `.png`, `.jpg`, or `.jpeg` images through the web interface.

The Flask backend:

1. Validates the uploaded file.
2. Temporarily saves it to the server.
3. Processes it using OpenCV.
4. Passes it to the trained CNN.
5. Returns the classification and confidence score to the browser.
6. Deletes the uploaded image after inference.

The application limits uploaded files to a maximum size of **16 MB**.

### Prediction Interface

The front end displays either:

- **Wildfire Detected**
- **No Wildfire Detected**

alongside the model's confidence score.

If a wildfire is detected, a coordinate-entry form becomes available.

### Wildfire Mapping

Following a positive prediction, users can manually enter:

- Latitude
- Longitude

The application stores the reported coordinates and uses **Folium** to generate an interactive map containing markers for each recorded wildfire location.

The interface also displays a list of recorded coordinates.

> **Note:** The CNN detects wildfire presence in an image but does not determine the image's geographic location. Coordinates must currently be entered manually by the user.

## Backend Routes

The Flask application includes several routes:

### `/`
Loads the main web interface.

### `/predict`
Receives an uploaded image, preprocesses it, runs CNN inference, and returns:

- Prediction
- Confidence score

### `/save_coordinates`
Stores user-provided latitude and longitude values following a positive detection.

### `/get_locations`
Returns the currently recorded wildfire coordinates.

### `/get_map`
Serves the interactive Folium map generated from the recorded coordinates.

## Deployment

The application was configured for deployment on **Render**.

Render installs the project's Python dependencies using:

```bash
pip install -r requirements.txt
```

The application is served using **Gunicorn**:

```bash
gunicorn app:app --bind 0.0.0.0:$PORT
```

The deployment configuration specifies **Python 3.11.8**.

## Running the Project Locally

### 1. Clone the repository

```bash
git clone <YOUR-REPOSITORY-URL>
cd Wildfire_Watch_Project
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Run the Flask application

```bash
python app.py
```

### 4. Open the application

Navigate to the local address displayed in your terminal, typically:

```text
http://127.0.0.1:5000
```

## Limitations

This project is an **introductory prototype**, not an operational wildfire-monitoring system.

Important limitations include:

- The model can produce false positives when presented with images substantially different from the satellite imagery used during development.
- Non-satellite images with strong visual features, such as highly red or green images, may be incorrectly classified.
- Images must currently be uploaded manually.
- Geographic coordinates must be entered manually following a positive detection.
- The model itself does not geolocate detected fires.
- Wildfire coordinates are stored in memory rather than in a persistent database.
- The original model-training notebook or training script is not preserved in this repository.
- The repository therefore contains the deployed model architecture and trained weights, but does not reproduce the original training process from scratch.
- A production system would require substantially more rigorous model validation and operational safeguards.

## Future Improvements

Potential next steps include:

- Automatically ingest satellite imagery rather than requiring manual uploads
- Associate imagery with geographic metadata automatically
- Add automated wildfire alerts following high-confidence detections
- Improve detection of out-of-distribution and non-satellite images
- Evaluate performance using clearly separated training, validation, and test datasets
- Report precision, recall, F1 score, and confusion matrices
- Compare the CNN against alternative architectures and pretrained vision models
- Add persistent database storage for wildfire locations
- Improve the user interface for viewing historical detections
- Integrate predictions with additional wildfire sensors or data sources

## Project Context

This project was developed as an introductory exploration of artificial intelligence and its potential applications to environmental challenges.

The project provided hands-on experience moving from an environmental problem to an end-to-end technical prototype involving:

- Image preprocessing
- Convolutional neural networks
- Model inference
- Python backend development
- Browser-based user interfaces
- API-style communication between a front end and backend
- Geospatial visualization
- Deployment configuration
- Evaluation of model limitations

The goal was not to develop a production-ready wildfire detection system, but to gain experience applying machine-learning concepts to a real environmental problem and integrating a trained model into a functional application.

## Project Materials

Additional project documentation will be added here, including:

- Technical white paper
- Project presentation
- Application screenshots

## Author

**Gavyn Oh**  
Duke University

Environmental Science & Policy  
Spatial Ecology & Environmental Data Sciences (SEEDS) Lab
