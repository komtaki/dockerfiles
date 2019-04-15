FROM  google/cloud-sdk:alpine

WORKDIR /root/

RUN apk update  \
    && apk upgrade  \
    && gcloud components install kubectl \
    && apk add docker --no-cache

ENTRYPOINT bash
