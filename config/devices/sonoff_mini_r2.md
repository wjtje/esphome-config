# The Sonoff MINI R2

> An ESPHome config for the Sonoff MINI R2 with extra features

## Features

- **Direct Mode**: The switch will directly change the relay
- **Double Toggle**: Detect a quick double toggle
- **Multiple Button Support**: 'Easly' add an extra input to your sonoff

## Basic config

```yaml
substitutions:
  node_id: sonoff-scullery # See esphome:name
  node_name: Sonoff Scullery # See esphome:friendly_name

  # Passwords
  encryption_key: "<GenerateMe!>"
  ota_password: "<GenerateMe!>"
  web_password: "ChangeME!"
  ap_password: "ChangeME!"
  wifi_ssid: !secret wifi_ssid
  wifi_password: !secret wifi_password

  # Settings
  relay_restore_mode: 'RESTORE_DEFAULT_OFF' # or ALWAYS_OFF, ALWAYS_ON
  direct_mode: '0' # -1 to disable, else toggle amount
  double_toggle: 'false'
  toggle_duration: '250' # Time in ms

packages:
  device: github://wjtje/esphome-config/config/devices/sonoff_mini_r2.yaml@main

# Manual IP config:
# wifi:
#   manual_ip: 
#     static_ip: 10.0.1.24
#     gateway: 10.0.0.1
#     subnet: 255.255.0.0
```

- **relay_restore_mode**: Control how the relay attempts to restore state on bootup. ([docs](https://esphome.io/components/switch/#config-switch))
- **direct_mode**: This enables or disables the direct mode feature. When the API is not connected direct mode is always enabled.
- **double_toggle**: This enables or disables the double toggle feature.
- **toggle_duration**: The maximum time between two toggles for triggering a double toggle.

## Home Assistant events

When performing a single or double toggle an event is sent to Home Assistant. You can use the following trigger inside automation to listen to the event.

```yaml
platform: event
event_type: esphome.sonoff_toggle
event_data:
  node_id: sonoff-mini-r2
  button_id: 0
  toggle_amount: 0

# For a single toggle
platform: event
event_type: esphome.sonoff_single_toggle
event_data:
  node_id: sonoff-mini-r2
  button_id: 0


# For a double toggle
platform: event
event_type: esphome.sonoff_double_toggle
event_data:
  node_id: sonoff-mini-r2
  button_id: 0
```

## Extra button

To add support for an extra button, you need to add this extra code in your config file. You will need to update the button_id to a higher number, the default button always has id 0.

```yaml
binary_sensor:
  # The external switch
  - platform: gpio
    name: ${node_name} Switch 1
    pin: GPIO14
    id: switch_1
    filters:
      - invert:
      - delayed_off: 10ms
    on_state:
      then:
        - script.execute:
            id: on_switch_toggle_v2
            button_id: 1
        # Or if you want different settings for this switch
        # - script.execute:
        #     id: on_switch_toggle
        #     button_id: 1
        #     direct_mode: 
        #     double_toggle:
        #     double_toggle_time:
```

## Notes

When the double toggle is enabled, there is a small delay between the first toggle and sending the single toggle event or enabling the relay.

It's possible to trigger events based on the state of the binary sensor, but this is not recommended to do.

## License

MIT License

Copyright (c) 2024 Wouter
