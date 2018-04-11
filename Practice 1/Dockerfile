FROM alpine:3.4
MAINTAINER 
# Run a few package install for test purpose
RUN apk update
RUN apk add vim
RUN apk add curl
RUN apk add git

# In order to minimize cache images created from each RUN
# put them into a single command
# RUN apk update && apk add vim curl git
