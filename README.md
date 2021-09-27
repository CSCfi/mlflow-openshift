# MLflow deployment for OpenShift container cloud

This repository contains OpenShift Template of [MLflow Tracking server](https://mlflow.org/docs/latest/tracking.html) and [MLflow models](https://mlflow.org/docs/latest/models.html).  
**MLflow Tracking Server** records data and metrics to track and compare performance of machine learning training experiments.  
**MLflow Models** is used to deploy machine learning models for inference.  

With default settings services are accessible from everywhere. To restrict access modify Whitelist variable
in OpenShift template.  

MLflow Models service is deployed but not started. Starting up Models serving pod needs access to model stored in Rahti PVC storage.
You can start Models after setting up
`MODELS_URI` in `models-cfg` config map by increasing pod count to 1. 

Different backend options:
- Default backend for metrics and artifacts is PVC-storage in Rahti project. That is easiest way to use this template.
- Other option to store metrics is to use database. MySQL template can be found from Rahti catalog to use for that purpose. (instructions TBA) 

Since artifacts can be quite large files, it might be necessary to change `DEFAULT_ARTIFACT_ROOT` to point some external storage system.
- Default artifact store is set to `./mlruns` to avoid errors in autolog-functions. That causes artifacts to be stored to local `./mlruns` directory in machine executing training. These artifacts cannot be seen in tracking server.
- CSC Allas or some other S3 compatible object storage should be set up for artifact store to make them available to Tracking server. (instructions TBA)

## Things to note

Models pod is scaled to 0 initially. It cannot be started unless working model_uri
is passed to `models-cfg` config map. Model should be first developed externally and 
stored into MLflow Model Registry from Tracking server ui to be used for Models Serving

Please note that MLflow itself is not programming environment. You can develop your machine learning code in your own environment 
in your own machine or in CSC cloud or supercomputer services.
You should set up environment variables in your programming environment to access this MLflow Tracking server!
Tracking server uri is the address of your deployed tracking server (for example: https://<APP_NAME>.rahtiapp.fi)
```bash
export MLFLOW_TRACKING_URI=your_address
export MLFLOW_TRACKING_USERNAME=your_username
export MLFLOW_TRACKING_PASSWORD=your_password
```

---

## Version history
**Version 0.6.0 - 24.9.2021
Distribute metrics to PostgreSQL**
- Added postgres and psycopg2 libraries to Dockerfile
- Added PostgreSQL pod deployment to template - no startup
- Added secrets, pvc and service for postgresql
- Reduced mlflow-ui storage from 10gi to 1gi
- Changed startup-script to default artifact store to ./mlruns
- Default backend-store still pvc-filesystem
- Default artifact store set to ./mlruns and note to instructions

**Version 0.5.1 - 20.9.2021
Update a bit**
- Updated MLflow from 1.13.1 -> 1.20.2
- Python image source back to DockerHub

**Version 0.5.0 - 18.1.2021
Let's get back to basics**
- Removed Helm chart from master
- Removed Allas configuration and instructions from master to make it simpler to setup 
- Created new 'dev' branch for advanced features

**Version 0.4.0 - 27.11.2020
Towards public template**
- Upgraded MLflow to 1.12.0 version
- Changed configuration to read S3 credentials from Secret to env var 
- alpine-htpasswd image source from DockerHub to Rahti Image registry
- README improvements
- Changed template structure

**Version 0.3.0 - 13.11.2020  
From tracking only to serving models:**
- Added Models serving to both Helm chart and template

Known issue: S3 credentials still set in deployment env vars instead of secret

**Version 0.2.0 - 11.11.2020  
Towards multi-purpose receipt:**
- Upgraded MLflow to 1.11.0 version
- Public image from Rahti Registry set as default
- Added OpenShift template for Rahti
- Changes made to prepare for separating Tracking Server and Models to different pods
- Minor fixes and cleanings

**Version 0.1.0 - 9.3.2020  
Towards Public networks:**
- Added support for IP-whitelisting
- Added simple NGINX-proxy authentication


*Created by Juha Hulkkonen*
