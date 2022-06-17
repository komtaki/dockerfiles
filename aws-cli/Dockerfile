FROM python:3.9-slim

ARG pip_installer="https://bootstrap.pypa.io/get-pip.py"

RUN mkdir /data
RUN chmod -x /data

ENV PATH $PATH:/root/.local/bin

# Install awscli
RUN pip install awscli

# Install sam
RUN pip install --user --upgrade aws-sam-cli

# install command.
RUN apt-get update && apt-get install -y less vim

WORKDIR /root