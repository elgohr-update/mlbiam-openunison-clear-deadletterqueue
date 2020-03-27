FROM ubuntu:18.04

MAINTAINER Tremolo Security, Inc. - Docker <docker@tremolosecurity.com>

ENV JDK_VERSION=1.8.0 \
    OPENUNISON_VERSION="1.0.17.1"

LABEL io.k8s.description="Job to clear dead letter queues" \
      io.k8s.display-name="OpenUnison Dead Letter Queue Clearer" 

RUN apt-get update;apt-get -y install curl openjdk-8-jdk-headless ;apt-get -y upgrade;apt-get clean;rm -rf /var/lib/apt/lists/*; \ 
    mkdir -p /usr/local/openunison && \
    groupadd -r openunison -g 433 && \
    useradd -u 431 -r -g openunison -d /usr/local/openunison -s /sbin/nologin -c "OpenUnison Docker image user" openunison && \
    curl https://nexus.tremolo.io/repository/releases/com/tremolosecurity/unison/openunison-util/$OPENUNISON_VERSION/openunison-util-$OPENUNISON_VERSION.jar > /usr/local/openunison/openunison-utils.jar && \
    chown -R openunison:openunison /usr/local/openunison 


USER 431


