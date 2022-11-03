FROM python:3.7.0

LABEL maintainer="Tuomas Ohtonen <tuomas.ohtonen@csc.fi>"

RUN pip install --upgrade pip
## Install optional dependencies:
## boto3 for S3 connections, sklearn and pyspark for serving models
RUN pip install h5py
RUN pip install boto3
RUN pip install scikit-learn
RUN pip install tensorflow==2.0.0
## RUN pip install pyspark
RUN pip install mysqlclient
RUN pip install postgres
RUN pip install --no-binary :all: psycopg2

## Install MLflow
RUN pip install mlflow==1.30.0

## Start it up
CMD mlflow
