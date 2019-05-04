FROM ubuntu:14.04

ENV DEBIAN_FRONTEND noninteractive

ARG NGINX_VERSION

RUN apt-get update \
  && apt-get install -y \
  nginx=${NGINX_VERSION} \
  vim \
  unzip \
  zip \
  curl

COPY ./default.conf /etc/nginx/sites-enabled/default

WORKDIR /var/www/html/current

ENTRYPOINT nginx -g "daemon off;"