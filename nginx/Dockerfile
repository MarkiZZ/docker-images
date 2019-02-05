FROM nginx:alpine

USER root

RUN apk --no-cache add shadow; \
usermod -u 1000 -d /srv nginx; \
groupmod -g 1000 nginx

WORKDIR /srv