# Changelog

## 2.0.0

**BREAKING for this instance's entities — do not update casually; follow the cutover runbook (`~/Code/forks/floor2-cutover-runbook.html`).** Entity `unique_id`s and MQTT topics change from the old LFDI-suffix scheme to `device_name`-prefixed ones (e.g. `xcel_itron_5_floor_2_current_summation_delivered_value`).

- Retire the private `austindotzip/xcel_itron2mqtt` python fork: build **stock** [zaknye/xcel_itron2mqtt](https://github.com/zaknye/xcel_itron2mqtt) at a pinned SHA (`5cefeca`, current main) plus a single build-time patch (`device_name.patch`) adding the `DEVICE_NAME` env var — pending upstream as [zaknye/xcel_itron2mqtt#52](https://github.com/zaknye/xcel_itron2mqtt/pull/52). Once merged, bump the SHA and delete the patch.
- New **required** `device_name` option (default `Xcel Itron 5 Floor 2`), exported as `DEVICE_NAME`. A distinct name per meter is what prevents unique_id/topic collisions with the primary meter.
- Sync add-on scaffolding with upstream [wingrunr21/hassio-xcel-itron-mqtt](https://github.com/wingrunr21/hassio-xcel-itron-mqtt): uv-based dependency install from upstream's lockfile, base-python 18.0.0, `mqtt`/`loglevel` options, `ldfi` → `lfdi` config migration.
- Drop the vendored `openssl.conf` workaround — modern zaknye configures the CCM8 cipher and legacy renegotiation in-process.
- New sensors on firmware 3.2.50: VAh/VARh, TOU 0–3 WH, Max Demand, Power Factor (overall + per-phase).

## 1.4.1

- Add log output for MQTT and Meter configuration prior to running

## 1.4.0

- Downgrade [hassio-addons/base-python](https://github.com/hassio-addons/addon-base-python) to 13.1.3 to address OpenSSL issue

## 1.3.2

- vendor OpenSSL config to try and get OpenSSL to cooperate with the meter again

## 1.3.1

- Lots of documentation updates
- Fix OpenSSL config to allow unsafe ciphers again

## 1.3.0

- Update [zaknye/xcel_itron2mqtt](https://github.com/zaknye/xcel_itron2mqtt) to change `timePeriod_duration` to `duration` device class (via [#12](https://github.com/wingrunr21/hassio-xcel-itron-mqtt/pull/12))
- Add default `BUILD_FROM` argument to `Dockerfile`
- Bump [hassio-addons/base-python](https://github.com/hassio-addons/addon-base-python) to 15.0.1

## 1.2.1

- Fix Dockerfile to maintain upstream directory structure

## 1.2.0

- Update [zaknye/xcel_itron2mqtt](https://github.com/zaknye/xcel_itron2mqtt) to address missing `touTier` from [zaknye/xcel_itron2mqtt#25](https://github.com/zaknye/xcel_itron2mqtt/pull/25)
- Bump [hassio-addons/base-python](https://github.com/hassio-addons/addon-base-python) to 13.1.3

## 1.1.0

- Update [zaknye/xcel_itron2mqtt](https://github.com/zaknye/xcel_itron2mqtt) to use retry functionality from [zaknye/xcel_itron2mqtt#24](https://github.com/zaknye/xcel_itron2mqtt/pull/24)
- Bump [hassio-addons/base-python](https://github.com/hassio-addons/addon-base-python) to 13.1.1

## 1.0.1

- Fix `libssl3` and `libcrypto3` standalones conflicting with OpenSSL
- Bump [hassio-addons/base-python](https://github.com/hassio-addons/addon-base-python) to to 13.0.0

## 1.0.0

- Initial release
