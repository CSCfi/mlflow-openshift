FROM python:3.7.0

LABEL maintainer="Juha Hulkkonen <juha.hulkkonen@csc.fi>"

## Install optional dependencies:
## boto3 for S3 connections, sklearn and pyspark for serving models
RUN pip install boto3
RUN pip install scikit-learn
RUN pip install pyspark
RUN pip install mysqlclient

## Install MLflow
RUN pip install mlflow==1.20.2

## Start it up
CMD mlflow
