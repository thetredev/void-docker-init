# image/Dockerfile has to be built with the tag void-docker-runit:base to make this one work.
FROM void-docker-runit:base

RUN xbps-install -Sy shadow && \
    xbps-install -Sy nginx && \
    ln -s /etc/sv/nginx /var/service

# sleep forever in the foreground
COPY ./entrypoint.sh /
