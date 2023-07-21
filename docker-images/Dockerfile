FROM ghcr.io/mlflow/mlflow:v2.5.0

LABEL maintainer="Juha Hulkkonen <juha.hulkkonen@csc.fi>"

RUN pip install --upgrade pip
## Install optional dependencies:
## boto3 for S3 connections, sklearn and tensorflow for serving models
RUN pip install h5py
RUN pip install boto3 #==1.24.28
RUN pip install defusedxml
RUN pip install pyopenssl
RUN pip install scipy #==1.10.1
RUN pip install scikit-learn
RUN pip install tensorflow #==2.11.1
RUN pip install postgres
RUN pip install psycopg2-binary
RUN pip install fsspec
RUN pip install packaging #==23.0
RUN pip install pandas #==1.5.3
RUN pip install pillow #==9.4.0
RUN pip install requests #==2.28.2
RUN pip install psutil


## Start it up
CMD mlflow
