
# Application architecture #

Following services (pods) will be created:  
- **MLflow Tracking Server** records data and metrics to track and compare performance of machine learning training experiments.  
- **MLflow Models** is used to serve machine learning model for inference. This service is not included in tutorial for now.
- **PostgreSQL** database to store all metrics and metadata from your training runs.  
- **Minio** object storage to work as easy to approach artifact store. CSC Allas is preferred for heavier use.

## Things to note ##

Artifact storage:
- Minio object storage in path `s3://default` is created to work as Default artifact store. This is good for testing and small scale usage.
- Since artifacts can be quite large files, it might be necessary to change `DEFAULT_ARTIFACT_ROOT` to point some external storage system. CSC Allas or some other external S3 compatible object storage should be set up for artifact store to make them available to Tracking server.  [Instructions in User Guide](./docs/USER_GUIDE.md#using-csc-allas-as-artifact-store)

To utilize artifact store, you have to add generated keys to your programming environment. Keys are shown on summary page when deploying template.
After deployment, keys can be found from Rahti web console `Resources -> Secrets` view. Example below in **Variables for programming environment** -section.

With default settings services are accessible from everywhere. To restrict access modify Whitelist variable
in OpenShift template and add your static ip or your organization ip range.  

MLflow Models is deployed but not started. Starting up Models serving pod needs access to model stored in Artifact store.
You can start Models after setting up
`MODELS_URI` in `models-cfg` config map by increasing pod count to 1 after you have saved your model to MLflow model registry. [Instructions in User Guide](./docs/USER_GUIDE.md#mlflow-models)

### Variables for programming environment ###
Please note that MLflow itself is not programming environment. You can develop your machine learning code in your own environment 
in your own machine or in CSC cloud or supercomputer services.
You should set up environment variables in your programming environment to access this MLflow Tracking server!
Tracking server uri is the address of your deployed tracking server (for example: https://<YOUR_APP_NAME>.rahtiapp.fi)
AWS keys are same as Artifact store keys or your Allas access keys.
```bash
export MLFLOW_TRACKING_URI=https://<YOUR_APP_NAME>.rahtiapp.fi
export MLFLOW_TRACKING_USERNAME=your_username
export MLFLOW_TRACKING_PASSWORD=your_password

export MLFLOW_S3_ENDPOINT_URL=https://<YOUR_APP_NAME>-minio.rahtiapp.fi
export AWS_ACCESS_KEY_ID=your_generated_access_key
export AWS_SECRET_ACCESS_KEY=your_generated_secret_key 
```
OR with Conda based environment
```bash
conda env config vars set MLFLOW_TRACKING_URI=https://<YOUR_APP_NAME>.rahtiapp.fi
conda env config vars set MLFLOW_TRACKING_USERNAME=your_username
conda env config vars set MLFLOW_TRACKING_PASSWORD=your_password

conda env config vars set MLFLOW_S3_ENDPOINT_URL=https://<YOUR_APP_NAME>-minio.rahtiapp.fi
conda env config vars set AWS_ACCESS_KEY_ID=your_generated_access_key
conda env config vars set AWS_SECRET_ACCESS_KEY=your_generated_secret_key 
```