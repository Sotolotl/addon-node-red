ARG BUILD_FROM=hassioaddons/debian-base:3.2.1
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Copy Node-RED package.json
COPY package.json requirements.txt /opt/

# Set workdir
WORKDIR /opt

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Setup base
RUN \
    apk add --no-cache --virtual .build-dependencies \
        g++=9.3.0-r2 \
        gcc=9.3.0-r2 \
        libc-dev=0.7.2-r3 \
        linux-headers=5.4.5-r1 \
        make=4.3-r0 \
        py3-pip=20.1.1-r0 \
        python3-dev=3.8.5-r0 \
    \
    && apk add --no-cache \
        git=2.26.2-r0 \
        lua-resty-http=0.15-r0 \
        nginx-mod-http-lua=1.18.0-r0 \
        nginx=1.18.0-r0  \
        nodejs=12.18.3-r0 \
        npm=12.18.3-r0 \
        openssh-client=8.3_p1-r0 \
        patch=2.7.6-r6 \
        python3=3.8.5-r0 \
    \
    && npm config set unsafe-perm true \
    \
    && pip install --no-cache-dir -r /opt/requirements.txt \
    \
    && npm install \
        --no-audit \
        --no-optional \
        --no-update-notifier \
        --only=production \
        --unsafe-perm \
    \
    && npm cache clear --force \
    \
    && echo -e "StrictHostKeyChecking no" >> /etc/ssh/ssh_config \
    \
    && apk del --no-cache --purge .build-dependencies \
    && rm -fr \
        /tmp/* \
        /etc/nginx

# Copy root filesystem
COPY rootfs /

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_REF
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="Node-RED" \
    io.hass.description="Flow-based programming for the Internet of Things" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Franck Nijhof <frenck@addons.community>" \
    org.opencontainers.image.title="Node-RED" \
    org.opencontainers.image.description="Flow-based programming for the Internet of Things" \
    org.opencontainers.image.vendor="Home Assistant Community Add-ons" \
    org.opencontainers.image.authors="Franck Nijhof <frenck@addons.community>" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.url="https://addons.community" \
    org.opencontainers.image.source="https://github.com/hassio-addons/addon-node-red" \
    org.opencontainers.image.documentation="https://github.com/hassio-addons/addon-node-red/blob/master/README.md" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}
