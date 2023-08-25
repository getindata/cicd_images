FROM python:3.10 as base
RUN apt-get update && \
    apt-get install ca-certificates openssl wget docker.io jq -y

# Add gitlab.com as known host for dp publish command
RUN ssh-keyscan -t rsa gitlab.com >> ~/.ssh/known_hosts

# Update python
RUN pip install --upgrade pip setuptools wheel --no-cache-dir

FROM base as gcp
COPY requirements-gcp.txt ./
RUN pip install -r requirements-gcp.txt
RUN wget https://dl.google.com/dl/cloudsdk/release/google-cloud-sdk.tar.gz && \
    tar zxvf google-cloud-sdk.tar.gz && ./google-cloud-sdk/install.sh --usage-reporting=false --path-update=true && \
    rm -rf google-cloud-sdk.tar.gz

ENV PATH=$PATH:$PWD/google-cloud-sdk/bin/:/usr/local/bin/

FROM base as aws
RUN apt-get install awscli -y
# ENV PATH=$PATH:~/.local/bin
COPY requirements-aws.txt ./
RUN pip install -r requirements-aws.txt


FROM base as onprem
COPY requirements-onprem.txt ./
RUN pip install -r requirements-onprem.txt
