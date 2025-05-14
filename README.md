# Flask Application with CI/CD Pipeline

This repository contains a simple Flask application with a CI/CD pipeline that builds and pushes Docker images to Google Artifact Registry. The project uses Poetry for dependency management.

## Prerequisites

1. Google Cloud Platform (GCP) account
2. GitHub account
3. Docker installed locally (for development)
4. Poetry installed locally (for development)

## Setup Instructions

1. Install Poetry (if not already installed):
   ```bash
   curl -sSL https://install.python-poetry.org | python3 -
   ```

2. Create a Google Artifact Registry repository:
   ```bash
   gcloud artifacts repositories create flask-app \
       --repository-format=docker \
       --location=us-central1 \
       --description="Docker repository for Flask application"
   ```

3. Create a Service Account in GCP with the following roles:
   - Artifact Registry Writer
   - Storage Admin

4. Generate a JSON key for the Service Account and save it securely

5. Add the following secrets to your GitHub repository:
   - `GCP_PROJECT_ID`: Your Google Cloud Project ID
   - `GCP_SA_KEY`: The JSON key content of your Service Account

## Local Development

1. Install dependencies:
   ```bash
   poetry install
   ```

2. Run the application:
   ```bash
   poetry run python app.py
   ```

3. Build and run with Docker:
   ```bash
   docker build -t flask-app .
   docker run -p 5000:5000 flask-app
   ```

## CI/CD Pipeline

The GitHub Actions workflow will automatically:
1. Set up Python and Poetry
2. Install dependencies
3. Build the Docker image
4. Push it to Google Artifact Registry
5. Output the image URL

The workflow triggers on:
- Push to main branch
- Pull requests to main branch

## Accessing the Application

Once deployed, the application will be available at:
- Local: http://localhost:5000
- Container: http://localhost:5000 (when running with Docker) 