ARG DEBIAN_FRONTEND=noninteractive
ARG ARTEMIS_GIT_REPOSITORY=https://github.com/kit-sdq/Artemis
ARG ARTEMIS_VERSION=main

####################
# Build stage      #
####################
FROM openjdk:16-jdk-buster AS build

ENV LC_ALL C

RUN echo "Installing prerequisites" \
  && apt-get update && apt-get install -y --no-install-recommends curl \
  && curl -sL https://deb.nodesource.com/setup_15.x | bash - \
  && apt-get update && apt-get install -y --no-install-recommends \
  git \
  nodejs \
  rsync \
  && npm install --global yarn

ARG ARTEMIS_VERSION
ARG ARTEMIS_GIT_REPOSITORY

RUN echo "Building frontend from $ARTEMIS_GIT_REPOSITORY" \
  && mkdir /build && cd /build \
  && git clone --depth 1 --branch $ARTEMIS_VERSION $ARTEMIS_GIT_REPOSITORY \
  && cd Artemis \
  && yarn install \
  && yarn run webpack:prod \
  && rm build/resources/main/static/stats.json build/resources/main/static/report.html \
  && rsync -a src/main/resources/public/ build/resources/main/static/public/

####################
# Execution stage  #
####################
FROM nginx

COPY defaults/nginx.conf /etc/nginx/nginx.conf
# See https://hub.docker.com/_/nginx Environment variables
COPY defaults/artemis.conf.template /etc/nginx/templates/artemis.conf.template
COPY --from=build /build/Artemis/build/resources/main/static /usr/share/nginx/html

RUN rm /etc/nginx/conf.d/default.conf

EXPOSE 80
