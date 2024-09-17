# Dev Containers + Cloud Run + GitHub Actions


## Overview

This repository contains a simple Flask application that demonstrates how to use [Dev Containers](https://containers.dev/), [Cloud Run](https://cloud.google.com/run), and [GitHub Actions](https://docs.github.com/en/actions).


## Pre-requisites

1. [Docker](https://docs.docker.com/get-docker/) installed on your local machine with at least 4GB of free resources.
2. A [GCP project](https://cloud.google.com/resource-manager/docs/creating-managing-projects).
3. A [GitHub account](https://github.com/).


## Run the application locally on a dev container using IntelliJ IDEA

For instructions on how to use a dev container with Visual Studio Code, check out [this tutorial](https://code.visualstudio.com/docs/devcontainers/tutorial).

1. Clone this repository to your development machine.
2. Select the `.devcontainer/devcontainer.json` file and right click.
3. Select _Dev Containers → Create Dev Container and Mount Sources_. To restart an existing container, you can select _Dev Containers → Show Dev Containers_ and choose a container to run.
4. **[Skip this step if you have already built the container]** After creating a dev container, select a JetBrains IDE from the list (you might only see one) and click _Continue_. The selected IDE will be downloaded and the client will connect to it. Note that building the dev container and installing the IDE can take a little while, but you only need to do this once.
5. Once ready, you will see a new window pop up with the IDE running in the dev container.
6. You can now run the application by opening the terminal in the dev container IDE and type the following command:
```sh  
python3 main.py
```
7. Open a browser window and navigate to the url shown in the terminal logs to see the application running locally. Alternatively, you can use the dropdown menu labeled _Your application is listening on port 8080:_ right on the terminal window, and select _Forward Port and Open in Browser_ to open the application in a browser window. If you see the message "Hello, World!" then the application is running successfully. If you see an error, make sure you are forwarding the port.


## Set Up Your Cloud Run Service

1. In the `devcontainer.json` file, update the `GOOGLE_CLOUD_PROJECT` and `CLOUD_RUN_SERVICE` environment variables with your GCP Project ID and the name of the Cloud Run service. You don't need to set up the service in advance, just pick a unique name. Rebuild the container.
2. Open a new terminal in the dev container IDE and run the following command to authenticate with GCP:
```sh
gcloud init
```
3. Click on the link and follow the sign-in prompts. Once finished, copy the verification code and paste it in the terminal.
4. **[Skip this step if you have already set up a repository in Artifact Registry]** Enable the Artifact Registry API on your GCP project and create a repository:
```sh
gcloud artifacts repositories create YOUR-GCR-ARTIFACTS-REPO
  --repository-format docker 
  --project YOUR-PROJECT-ID 
  --location us-central1
```
5. Build the container image and push it to Artifact Registry by running the following command:
```sh
gcloud builds submit --config=cloudbuild.yaml --project YOUR-PROJECT-ID .
```
6. Make sure your image appears in the [Artifact Registry](https://console.cloud.google.com/artifacts).
7. Test your app locally by running the following command:
```sh
pytest
```


## Deploy to Cloud Run Using GitHub Actions

1. In your GCP dashboard, create a service account and generate a key for it in JSON format. Give the service account the following roles: `artifactregistry.writer`, `run.admin`, and `iam.serviceAccountUser`.
2. In your GitHub repository settings, add the key to _Secrets & Variables → Actions → Repository Secrets_. Name it `GCP_KEY`.
3. Inside the `.github/workflows` directory you will find a yaml file named `cloud-run-deploy.yaml`. This file contains the required instructions to deploy the application every time you push to the main branch.
4. Push to the main branch and check the Actions tab in your GitHub repository.
5. Once the deployment is successful, you can access the application by clicking on the Cloud Run `Service URL` in the GitHub Actions logs.
