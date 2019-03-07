# docker-presence

Implements [andrewjfreyer's presence script](https://github.com/andrewjfreyer/presence). `presence` is bluetooth-based presence detection useful for mqtt-based home automation.

## Usage

Create the config files (owner_devices & mqtt_preferences) as described [here](https://github.com/andrewjfreyer/presence).
The script tries to create the `behavior_preferences` file but doesn't succeed, so create it yourself with these contents (default):

```
#DELAY BETWEEN SCANS OF OWNER DEVICES WHEN AWAY FROM HOME
delay_between_owner_scans_away=6

#DELAY BETWEEN SCANS OF OWNER DEVICES WHEN HOME
delay_between_owner_scans_present=30

#HOW MANY VERIFICATIONS ARE REQUIRED TO DETERMINE A DEVICE IS AWAY
verification_of_away_loop_size=6

#HOW LONG TO DELAY BETWEEN VERIFICATIONS THAT A DEVICE IS AWAY
verification_of_away_loop_delay=3

#PREFERRED HCI DEVICE
hci_device='hci0'
```

Run:

```bash
docker run --name presence -d \
  --restart always \
  - v /path/to/config/owner_devices:/presence/owner_devices:ro \
  - v /path/to/config/mqtt_preferences:/presence/mqtt_preferences:ro \
  - v /path/to/config/behavior_preferences:/presence/behavior_preferences \
  jordandrako/presence
```

Docker-compose:

```yml
version: "3"

services:
  presence:
    image: jordandrako/presence
    container_name: presence
    restart: always
    volumes:
      - /path/to/config/owner_devices:/presence/owner_devices:ro
      - /path/to/config/mqtt_preferences:/presence/mqtt_preferences:ro
      - /path/to/config/behavior_preferences:/presence/behavior_preferences
```