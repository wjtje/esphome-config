# ESPHome config

This are common configuration files I use for my devices running ESPHome.

## Devices

- [Sonoff Mini R2](config/devices/sonoff_mini_r2.md)

## Usefull command for debugging ESPhome on Home Assistant

These commands can be run in the 'Advanced SSH & Web Terminal' with protection mode disabled. **WARNING** If you don't know exactly what these commands do, DO NOT GO ANY FURTHER.

- `docker ps -a` To list all the running docker containers
- `docker inspect -f '{{ .Mounts }}' <addon_id>` To list the mounts of the addon
- `docker run -it --rm -v <path>:/data alpine` To get a quick shell where the /data with is bind to the mount of the other container
