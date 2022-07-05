ARG BUILD_FROM
FROM $BUILD_FROM

# Build libcec for HDMI-CEC
ARG LIBCEC_VERSION
WORKDIR /usr/src
RUN \
    apk add --no-cache \
        eudev-libs\
        p8-platform \
        raspberrypi-dev \
        raspberrypi-libs \
    && apk add --no-cache --virtual .build-dependencies \
        build-base \
        cmake \
        eudev-dev \
        git \
        p8-platform-dev \
        swig \
        linux-headers \
    && git clone --depth 1 -b libcec-${LIBCEC_VERSION} \
        "https://github.com/Pulse-Eight/libcec" libcec \
    && mkdir -p libcec/build \
    && cd libcec/build \
    && cmake \
        -DCMAKE_INSTALL_PREFIX:PATH=/usr/local \
        -DRPI_INCLUDE_DIR=/opt/vc/include \
        -DRPI_LIB_DIR=/opt/vc/lib \
        .. \
    && make -j$(nproc) \
    && make install \
    && apk del --no-cache .build-dependencies \
    && cp /opt/vc/lib/* /usr/local/lib/ \
    && rm -rf /usr/src/*

# Copy data
WORKDIR /
COPY rootfs /
