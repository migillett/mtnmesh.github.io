### Docker Volumes

If you're using [Meshtasticd via Docker Compose](https://hub.docker.com/r/meshtastic/meshtasticd), you can also map your node's configuration to a separate docker volume. That way you can flip between presets quickly if need be. You just have to uncomment the one you want and recreate the container. For example:

```yaml
services:
  meshtasticd:
    image: meshtastic/meshtasticd:alpha-debian
    container_name: meshtasticd
    devices:
      - "/dev/spidev0.0"
      - "/dev/gpiochip0"
      - "/dev/i2c-1:/dev/i2c-1"
    ports:
      - "4403:4403"
    cap_add:
      - SYS_RAWIO
    volumes:
      - ./config.yaml:/etc/meshtasticd/config.yaml:ro
#      - ./LongFast:/var/lib/meshtasticd # previous modem preset
      - ./MediumFast:/var/lib/meshtasticd # new modem preset
    restart: unless-stopped
```

You still have to configure each of the volumes from scratch, but it's a quick way of flip-flopping between presets if that's your vibe.
