# The Sonoff MINI R2

> An ESPHome config for the Sonoff MINI R2 with extra features

## Features

- **Direct Mode**: The switch will directly change the relay
- **Double Toggle**: Detect a quick double toggle
- **Multiple Button Support**: Easly add an extra input to your sonoff

## Basic config

```yaml
substitutions:
  node_id: sonoff-mini-r2
  node_name: "Sonoff MINI R2"

  encryption_key: "Your encryption key"
  password: "Your API/OTA/AP password"
  wifi_ssid: !secret wifi_ssid
  wifi_password: !secret wifi_password
  
  direct_mode: 'true'
  double_toggle: 'true'
  double_toggle_time: '0.20'
  relay_restore_mode: 'RESTORE_DEFAULT_OFF'

packages:
  device: github://wjtje/esphome-config/config/devices/sonoff_mini_r2.yaml@main
```

- **node_id** (*Required*, id): This is the esphome node_id, also used for sending events to Home Assistant.
- **node_name** (*Required*, string): The name is used for each entity to create unique names.
- **encryption_key** (*Required*, string): A base64 encryption key used for noise encryption.
- **password** (*Required*, string): A password used for the API, OTA, AP, and web_server. The default web_server username is admin.
- **direct_mode** (*Required*, `true` or `false`): This enables or disables the direct mode feature. When the API is connected direct mode is always enabled.
- **double_toggle** (*Required*, `true` or `false`): TThis enables or disables the double toggle feature.
- **double_toggle_time** (*Required*, number): The maximum time between two toggles for triggering a double toggle.
- **restore_mode**: (*Required*, string): Control how the relay attempts to restore state on bootup. ([docs](https://esphome.io/components/switch/gpio.html))

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
    pin: GPIO04
    id: switch_1
    filters:
      - invert:
    on_state:
      then:
        - script.execute:
            id: on_switch_toggle
            button_id: 1
            direct_mode: !lambda 'return id(direct_mode);'
            double_toggle: !lambda 'return id(double_toggle);'
            double_toggle_time: !lambda 'return id(double_toggle_time).state;'
```

## Notes

When the double toggle is enabled, there is a small delay between the first toggle and sending the single toggle event or enabling the relay.

It's possible to trigger events based on the state of the binary sensor, but this is not recommended to do.

## License

MIT License

Copyright (c) 2022 Wouter van der Wal
