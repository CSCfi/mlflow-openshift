# MLflow tracking server deployment for OpenShift container cloud

This template implements MLflow Tracking server and MLflow models serving to OpenShift.
By default settings MLflow Tracking server is deployed and started with access restrictions to use from CSC networks only. To change this modify Whitelist variable
in OpenShift template or `routeWhitelist` in Helm chart.
Starting up Models serving pod needs access to model stored in Rahti or externally to Allas object storage.

Different backend options:
- Default backend for metrics and artifacts is PVC-storage in Rahti project. That is easiest way to use this template.
- Other option to store metrics is to use database. MySQL template can be found from Rahti catalog to use for that purpose. (instructions TBA) 

Since artifacts can be quite large files, it might be necessary to change `DEFAULT_ARTIFACT_ROOT` to point some other storage system.
- CSC Allas or some other S3 compatible object storage can be set up for artifact store.

!!! note 

    You can change artifact root explicitly by each experiment so you don't have to necessarily change DEFAULT_ARTIFACT_ROOT 
    to S3 address but you have to set up credentials before using S3 storage.

Template asks for Allas credentials if you would like to use S3 connection to Allas as backend to store artifacts. You can left those credentials and 
`DEFAULT_ARTIFACT_ROOT` variable empty if you would not like to setup S3. 

Instructions how to use different storage options can be found from [user guid](./user_guide.md) 

**WIP version.**

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

## Things to note

Models pod is scaled to 0 initially. It cannot be started unless working model_uri (and S3 credentials if S3 as source) 
is passed to `models-cfg` config map.

Please note that environment variables should be used in training machine to access remote tracking server!
```bash
export MLFLOW_TRACKING_USERNAME=username
export MLFLOW_TRACKING_PASSWORD=password
```

This deployment adds user defined S3 credentials from values.yaml to MLflow server. If you use deployed MLflow 
from other location than inside the pod, you have to add also S3 credentials to that environment.

```bash
export MLFLOW_S3_ENDPOINT_URL=https://a3s.fi
export MLFLOW_TRACKING_USERNAME=<your_username>
export MLFLOW_TRACKING_PASSWORD=<your_password>
```

---
OpenShift template creates PVC storage automatically but deployment with Helm expects existing pvc.
If pvc not exist and you are using Helm create it by applying following definition:

```
apiVersion: "v1"
kind: "PersistentVolumeClaim"
metadata:
    name: "storage-pvc"
spec:
    accessModes:
      - "ReadWriteMany"
    resources:
      requests:
        storage: "10Gi"
``` 
And add that storage name ("storage-pvc" here) to `storageName` variable in values.yaml

*Created by Juha Hulkkonen*
