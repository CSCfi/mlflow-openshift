# MLflow deployment for OpenShift container cloud #

This repository contains [MLflow](https://mlflow.org) - machine learning lifecycle management tool template and instructions how to install and utilize it.
MLflow template is installed into CSC Rahti cointainer cloud and can be found from Rahti template registry at [https://rahti.csc.fi](https://rahti.csc.fi).
If you are unfamiliar with Rahti and how to get access, first check [Rahti's documentation](https://docs.csc.fi/cloud/rahti/)

MLflow is open source tool to manage machine learning lifecycle. That means it can be used to track, store and compare all 
training code, data, config, and results with [MLflow Tracking Server](https://mlflow.org/docs/latest/tracking.html). 
Projects can be packaged with [MLflow Projects](https://mlflow.org/docs/latest/projects.html) to reproducible 
format for sharing between environments. 
MLflow can store, and manage models in a central [Model Registry](https://mlflow.org/docs/latest/model-registry.html) and models can be deployed for 
diverse serving environments with [MLflow Models](https://mlflow.org/docs/latest/models.html).   

[VIDEO: MLflow - How to setup and start using in Rahti](https://video.csc.fi/media/t/0_2frjyzz9)

---

## Contents
- [Installation instructions](./docs/USER_GUIDE.md#installation-instructions)

- [Setting up Models](./docs/USER_GUIDE.md#mlflow-models)

- [Using CSC Allas as artifact store](./docs/USER_GUIDE.md#using-csc-allas-as-artifact-store)

- [Examples as Jupyter notebook](./examples/README.md)

- [Version history](./README.md#version-history)

---

## Version history
**Version 0.9.2** 1.12.2021
**Expanding instruction horizon**
- Fixes to documentation
- Including link to first video tutorial

**Version 0.9.1** - 25.11.2021
**Usability improvements**
- Redesigned the documentation

**Version 0.9.0** - 8.11.2021  
**Up to 1.21**
- Upgradede MLflow from 1.20.2 -> 1.21.0
- Added ConfigChange trigger to models pod
- Changed MLFLOW_S3_ENDPOINT_URL from S3 secret to mlflow-cfg for ease change
- Improvements to README's and USER_GUIDE

**Version 0.8.1** - 26.10.2021  
**Improving the documentation**
- Links and minor additions and fixes to README.md
- File hierarchy clarified
- Links and improvements to example files and USER_GUIDE.md

**Version 0.8.0** - 22.10.2021  
**Lowering the learning curve**
- Added mlflow-secret.yaml for CSC Allas configuration
- Added user_guide.md with instructions for Models and CSC Allas
- Renamed tracking example
- Added inference example 
- Added README.md for examples

**Version 0.7.0** - 12.10.2021  
**Utilizing PostgreSQL and Minio**
- Included S3 credentials as secret and mounted for pods
- Added Minio and configured as default artifact store
- Defaulted PostgreSQL as backend store
- Unnecessary user defined parameters removed 
- Small changes to component names
- Added tensorflow 2.0.0 and h5py libraries to Mlflow image
- Removed user defined object store address from template and set generated credentials (instructions to modify TBA) 
- Improved documentation
- Added simple example notebook to demonstrate tracking metrics

**Version 0.6.0** - 24.9.2021  
**Distribute metrics to PostgreSQL**
- Added postgres and psycopg2 libraries to Dockerfile
- Added PostgreSQL pod deployment to template - no startup
- Added secrets, pvc and service for postgresql
- Reduced mlflow-ui storage from 10gi to 1gi
- Changed startup-script to default artifact store to ./mlruns
- Default backend-store still pvc-filesystem
- Default artifact store set to ./mlruns and note to instructions

**Version 0.5.1** - 20.9.2021  
**Update a bit**
- Updated MLflow from 1.13.1 -> 1.20.2
- Python image source back to DockerHub

**Version 0.5.0** - 18.1.2021  
**Let's get back to basics**
- Removed Helm chart from master
- Removed Allas configuration and instructions from master to make it simpler to setup 
- Created new 'dev' branch for advanced features

**Version 0.4.0** - 27.11.2020  
**Towards public template**
- Upgraded MLflow to 1.12.0 version
- Changed configuration to read S3 credentials from Secret to env var 
- alpine-htpasswd image source from DockerHub to Rahti Image registry
- README improvements
- Changed template structure

**Version 0.3.0** - 13.11.2020  
**From tracking only to serving models:**
- Added Models serving to both Helm chart and template

Known issue: S3 credentials still set in deployment env vars instead of secret

**Version 0.2.0** - 11.11.2020  
**Towards multi-purpose receipt:**
- Upgraded MLflow to 1.11.0 version
- Public image from Rahti Registry set as default
- Added OpenShift template for Rahti
- Changes made to prepare for separating Tracking Server and Models to different pods
- Minor fixes and cleanings

**Version 0.1.0** - 9.3.2020  
**Towards Public networks:**
- Added support for IP-whitelisting
- Added simple NGINX-proxy authentication


*Created by Juha Hulkkonen*
