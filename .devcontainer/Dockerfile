# Use the official lightweight Python image.
# https://hub.docker.com/_/python
FROM python:3.11-slim

# Set environment variable. This can also be done in the .devcontainer.json file.
# ENV GOOGLE_CLOUD_PROJECT 'YOUR-PROJECT-ID'

# Install system dependencies.
RUN apt-get update && apt-get install -y \
    apt-transport-https \
    ca-certificates \
    procps \
    gnupg \
    curl \
    git \
    python3-pip

# Import the Google Cloud public key.
RUN curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | gpg --dearmor -o /usr/share/keyrings/cloud.google.gpg

# Add the Google Cloud SDK distribution URI as a package source.
RUN echo 'deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main' | tee -a /etc/apt/sources.list.d/google-cloud-sdk.list

# Update and install the gcloud CLI.
RUN apt-get update && apt-get install -y google-cloud-cli

# Install dev environment dependencies.
RUN pip install 'Flask==3.0.3' 'Werkzeug==3.0.3' 'pytest==8.2.0'
