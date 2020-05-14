# syntax = docker/dockerfile:1.0-experimental
FROM ubuntu:bionic

# To make it easier for build and release pipelines to run apt-get,
# configure apt to not require confirmation (assume the -y argument by default)
ENV DEBIAN_FRONTEND=noninteractive


RUN echo "APT::Get::Assume-Yes \"true\";" > /etc/apt/apt.conf.d/90assumeyes

RUN ["/bin/bash", "-c", "apt-get update && apt-get install -y --no-install-recommends \
  ca-certificates \
  curl \
  jq \
  git \
  iputils-ping \
  libcurl4 \
  libicu60 \
  libunwind8 \
  netcat \
  && rm -rf /var/lib/apt/lists/* \
  "]

ARG AZP_URL

WORKDIR /azp
COPY ./scripts/ ./scripts

RUN --mount=type=secret,id=mysecret cat /run/secrets/mysecret > .token \
  && export AZP_TOKEN_FILE=/azp/.token \
  && chmod +x ./scripts/install.sh \
  && chmod +x ./scripts/start.sh \
  && ./scripts/install.sh \
  && rm -f .token \
  && rm -f ./scripts/install.sh

