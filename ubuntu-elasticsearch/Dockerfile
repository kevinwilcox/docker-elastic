## use Ubuntu 18 as the base
FROM ubuntu:18.04

## update all the things
##   then install the free JRE and curl
##   then download elasticsearch from Elastic
##   then install it
##   then delete the installation package file
## a better way to do this is to use ADD with a
##   local .deb but that's not as portable in
##   most build scenarios
RUN apt-get -qq update && \
    apt-get -qq -y upgrade && \
    apt-get -qq -y install curl openjdk-11-jre-headless && \
    curl https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.3.0-amd64.deb --output elasticsearch-7.3.0.deb && \
    dpkg -i elasticsearch-7.3.0.deb && \
    rm elasticsearch-7.3.0.deb

## elasticsearch.yml is the basic config for elasticsearch
##   and sets paths, IPs to listen on, etc
COPY elasticsearch.yml /etc/elasticsearch/elasticsearch.yml

## make sure the elasticsearch user owns the relevant configs/locations
##   arguably owned by root but readable by the ES group
## this is done by default in the installer, I do it here
##   just to show how to do it since I use this for demos and instruction
##   and because we just copied this file in
RUN chown root:elasticsearch /etc/elasticsearch/elasticsearch.yml
RUN chown elasticsearch:elasticsearch /var/lib/elasticsearch
RUN chown elasticsearch:elasticsearch /var/log/elasticsearch

## make sure to run commands in the container as the
##   elasticsearch user -- otherwise it will try as root
USER elasticsearch

## let Docker know the container will offer services on
##   ports 9200 to 9300, inclusively
EXPOSE 9200:9300
