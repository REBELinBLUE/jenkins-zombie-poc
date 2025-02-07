FROM jenkins/inbound-agent

ARG TARGETARCH
ARG TINI_VERSION=v0.19.0

USER root

RUN apt-get update \
    && apt-get install -y procps

ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini-${TARGETARCH} /usr/local/bin/tini

RUN chmod +x /usr/local/bin/tini
