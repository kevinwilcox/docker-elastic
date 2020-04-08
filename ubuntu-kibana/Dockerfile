## use Ubuntu 18 as the base
FROM ubuntu:18.04

## update all the things
##   then install the free JRE and curl
##   then download kibana from Elastic
##   then install it
##   then delete the installation package file
## a better way to do this is to use ADD with a
##   local .deb but that's not as portable in
##   most build scenarios
RUN apt-get -qq update && \
    apt-get -qq -y upgrade && \
    apt-get -qq -y install curl openjdk-11-jre-headless && \
    curl https://artifacts.elastic.co/downloads/kibana/kibana-7.3.0-amd64.deb --output kibana-7.3.0.deb && \
    dpkg -i kibana-7.3.0.deb && \
    rm kibana-7.3.0.deb

## kibana.yml sets which IPs to listen on, paths, etc
COPY kibana.yml /etc/kibana/kibana.yml

## make sure the kibana user can read the config we just added
RUN chown root:kibana /etc/kibana/kibana.yml

## run everything as the kibana user, not as root
USER kibana

## tell Docker the container will offer services on
##   port 5601; it still needs to be published at run-time
EXPOSE 5601
