ARG BUILD_FROM="ghcr.io/hassio-addons/base-python:18.0.0"
FROM $BUILD_FROM

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Bring in the uv binary to install dependencies (upstream migrated pip -> uv in v1.2.0)
COPY --from=ghcr.io/astral-sh/uv:0.11.25 /uv /uvx /usr/local/bin/

# STOCK zaknye/xcel_itron2mqtt, pinned. The only delta from stock is
# device_name.patch (the DEVICE_NAME feature), which is pending upstream as
# https://github.com/zaknye/xcel_itron2mqtt/pull/52 — once that merges, bump
# this SHA past the merge and delete the patch + the COPY/patch lines below.
ARG XCEL_ITRON2MQTT_SHA=5cefecadfd833587163de8f88fcdc000aedb1d9b

COPY device_name.patch /tmp/device_name.patch

# Copy python source from upstream, apply the DEVICE_NAME patch, and install
# its locked dependencies
RUN apk add --no-cache patch && \
  curl -L https://github.com/zaknye/xcel_itron2mqtt/archive/${XCEL_ITRON2MQTT_SHA}.zip -o /tmp/main.zip && \
  cd /tmp && unzip /tmp/main.zip && \
  cd /tmp/xcel_itron2mqtt-${XCEL_ITRON2MQTT_SHA} && \
  patch -p1 < /tmp/device_name.patch && \
  mv /tmp/xcel_itron2mqtt-${XCEL_ITRON2MQTT_SHA}/xcel_itron2mqtt /opt && \
  mv /tmp/xcel_itron2mqtt-${XCEL_ITRON2MQTT_SHA}/pyproject.toml /opt/xcel_itron2mqtt/ && \
  mv /tmp/xcel_itron2mqtt-${XCEL_ITRON2MQTT_SHA}/uv.lock /opt/xcel_itron2mqtt/ && \
  rm /opt/xcel_itron2mqtt/run.sh && \
  rm -rf /tmp/main.zip /tmp/device_name.patch && \
  apk del patch libssl3 libcrypto3 && \
  apk add --no-cache openssl && \
  cd /opt/xcel_itron2mqtt && \
  uv sync --frozen --no-dev --no-install-project

# Run upstream out of the uv-managed virtualenv
ENV PATH="/opt/xcel_itron2mqtt/.venv/bin:$PATH"

# Copy root filesystem
COPY rootfs /

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_DESCRIPTION
ARG BUILD_NAME
ARG BUILD_REF
ARG BUILD_REPOSITORY
ARG BUILD_VERSION

# Labels
LABEL \
  io.hass.name="${BUILD_NAME}" \
  io.hass.description="${BUILD_DESCRIPTION}" \
  io.hass.arch="${BUILD_ARCH}" \
  io.hass.type="addon" \
  io.hass.version=${BUILD_VERSION} \
  org.opencontainers.image.title="${BUILD_NAME}" \
  org.opencontainers.image.description="${BUILD_DESCRIPTION}" \
  org.opencontainers.image.vendor="Home Assistant Community Add-ons" \
  org.opencontainers.image.licenses="MIT" \
  org.opencontainers.image.url="https://addons.community" \
  org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
  org.opencontainers.image.documentation="https://github.com/${BUILD_REPOSITORY}/blob/main/README.md" \
  org.opencontainers.image.created=${BUILD_DATE} \
  org.opencontainers.image.revision=${BUILD_REF} \
  org.opencontainers.image.version=${BUILD_VERSION}
