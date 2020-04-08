## use Ubuntu 18 as the base
FROM ubuntu:18.04

## update all the things
##   then install the free JRE and curl
##   then download logstash from Elastic
##   then install it
##   then delete the installation package file
## a better way to do this is to use ADD with a
##   local .deb but that's not as portable in
##   most build scenarios
RUN apt-get -qq update && \
    apt-get -qq -y upgrade && \
    apt-get -qq -y install curl openjdk-11-jre-headless && \
    curl https://artifacts.elastic.co/downloads/logstash/logstash-7.6.1.deb --output logstash-7.6.1.deb && \
    dpkg -i logstash-7.6.1.deb && \
    rm logstash-7.6.1.deb

## pipelines.yml tells logstash which pipelines
##   to build and which configs to use for each
## this config says use everything in /etc/logstash/conf.d
##   and put it in a pipeine called "main"
ADD pipelines.yml /etc/logstash/pipelines.yml

## logstash.yml is the basic config for logstash and
##   sets paths, some customisations, etc
ADD logstash.yml /etc/logstash/logstash.yml

## the simplest possible logstash testing config
##   type input into the console
##   match all the things
##   display to the console 
ADD basic_pipeline.conf /etc/logstash/conf.d/basic_pipeline.conf

## don't drop the user in as root, run things
##   as the logstash user created during the
##   above install
USER logstash

## this lets Docker know that the VM has a
##   service running on 5044; it will still
##   need to be published at run-time
EXPOSE 5044
