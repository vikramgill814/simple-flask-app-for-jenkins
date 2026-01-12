# Simple Flask Application for Jenkins CI/CD

This is a simple Flask application designed to demonstrate CI/CD deployment using Jenkins.

## Features

- Simple web application with a responsive UI
- Health check endpoint
- Displays confirmation message that it's deployed via Jenkins CI/CD pipeline

## Requirements

- Python 3.7+
- Flask 3.0.0

## Installation

1. Install dependencies:
```bash
pip install -r requirements.txt
```

## Running the Application

```bash
python app.py
```

The application will be available at `http://localhost:5000`

## Endpoints

- `GET /` - Main page displaying the deployment message
- `GET /health` - Health check endpoint returning JSON status

## Docker Deployment

To run this application in Docker:

```bash
docker build -t flask-jenkins-app .
docker run -p 5000:5000 flask-jenkins-app
```
