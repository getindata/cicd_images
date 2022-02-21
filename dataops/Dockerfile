FROM python:3.8-slim

ARG PLATFORM

RUN apt-get update && \
    apt-get install ca-certificates openssl wget docker.io -y

RUN pip install --upgrade pip wheel setuptools

COPY requirements.txt requirements-${PLATFORM}.tx[t] .
RUN pip install -r requirements.txt && \
    pip install -r requirements-${PLATFORM}.txt

RUN if [ "$PLATFORM" = "gcp" ]; then \
    wget https://dl.google.com/dl/cloudsdk/release/google-cloud-sdk.tar.gz && \
    tar zxvf google-cloud-sdk.tar.gz && ./google-cloud-sdk/install.sh --usage-reporting=false --path-update=true && \
    export PATH=$PATH:$PWD/google-cloud-sdk/bin/:/usr/local/bin/ && \
    rm -rf google-cloud-sdk.tar.gz ; \
    fi \

