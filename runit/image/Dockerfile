FROM ghcr.io/void-linux/void-linux:20220530RC01-thin-x86_64

ARG TINI_VERSION="v0.19.0"
ARG TINI_URL="https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini-amd64"

# update the system and XBPS
RUN xbps-install -Syu || true && \
    xbps-install -Syu xbps || true && \
    xbps-install -Syu || true \
    && \
        # install basic tools
        xbps-install -Sy \
            runit \
            bash \
            curl \
    && \
        # clean up XBPS
        xbps-remove -yOo \
    && \
        # prepare the system to make runit services run properly
        mkdir -p /etc/sv  && \
        mkdir -p /run/runit/runsvdir/current && \
        sed -i '/noextract=\/etc\/sv/d' /etc/xbps.d/noextract.conf \
    && \
        # install tini
        curl -fsSL --output /usr/local/bin/tini ${TINI_URL} && \
        chmod +x /usr/local/bin/tini \
    && \
        # set root login shell to bash
        sed -i 's/root:\/bin\/sh/root:\/bin\/bash/g' /etc/passwd

# set default shell to bash
SHELL [ "/bin/bash", "-c" ]
ENV SHELL="bash"

# copy runsvdir wrapper
COPY ./runsvdir-wrap /opt/.runsvdir-wrap

# copy default entrypoint
COPY ./entrypoint.sh /

# set tini to run runsvdir wrapper as entrypoint
ENTRYPOINT [ "/usr/local/bin/tini", "--", "/opt/.runsvdir-wrap" ]
