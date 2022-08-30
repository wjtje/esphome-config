# ESPHome config

This are common configuration files I use for my devices running ESPHome.

## Example config

```yaml
substitutions:
    node_id: test-node
    node_name: Test Node
    encryption_key: YOUR ENCRYPTION KEY
    password: A STRONG PASSWORD
    wifi_ssid: !secret wifi_ssid
    wifi_password: !secret wifi_password
    static_ip: 10.0.1.1

packages:
    device_base: github://wjtje/esphome-config/config/device_base.yaml@main
    static_ip: github://wjtje/esphome-config/config/static_ip.yaml@main
    default_components: github://wjtje/esphome-config/config/default_components.yaml@main
```
