# Xcel iTron MQTT (2) — second-meter instance

Renamed-slug wrapper around the upstream add-on so a **second** Xcel meter can run
alongside the primary one (Home Assistant allows only one add-on instance per
repository — renamed slugs were blessed by the upstream maintainer in
[wingrunr21/hassio-xcel-itron-mqtt#24](https://github.com/wingrunr21/hassio-xcel-itron-mqtt/issues/24)).

As of 2.0.0 this builds **stock**
[zaknye/xcel_itron2mqtt](https://github.com/zaknye/xcel_itron2mqtt) at a pinned SHA
plus a single build-time patch (`device_name.patch`) that adds the `DEVICE_NAME`
env var — pending upstream as
[zaknye/xcel_itron2mqtt#52](https://github.com/zaknye/xcel_itron2mqtt/pull/52).
When that merges: bump `XCEL_ITRON2MQTT_SHA` in the Dockerfile past the merge,
delete the patch and its two Dockerfile lines, and this add-on is 100% stock.

## Configuration

Same options as the upstream add-on, plus:

| Option        | Description | Required | Default |
| ------------- | ----------- | -------- | ------- |
| `device_name` | Home Assistant device name and prefix for every entity `unique_id`/MQTT topic. **Must differ from the primary meter's name** (the primary upstream instance uses `Xcel Itron 5`) — the distinct name is what prevents the two meters from colliding. | Yes | `Xcel Itron 5 Floor 2` |

⚠️ Changing `device_name` after entities exist changes every `unique_id` and
creates new entities — re-point the Energy Dashboard/automations or migrate the
entity registry (see the cutover runbook).

## Troubleshooting

### Summation Delivered Value Stops

This usually means your meter needs restarted. Email Xcel at
EnergyLaunchpad@xcelenergy.com and ask that they reboot your meter.
