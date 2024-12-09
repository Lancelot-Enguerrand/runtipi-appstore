
# JSON
```json
{
  "services": [
    {
      "name": "homeassistant",
      "image": "ghcr.io/home-assistant/home-assistant:stable",
      "isMain": true,
      "networkMode": "host",
      "volumes": [
        {
          "hostPath": "${APP_DATA_DIR}/config",
          "containerPath": "/config"
        }
      ],
      "privileged": true
    }
  ]
} 
```
# YAML
```yaml
version: '3'
services:
  homeassistant:
    container_name: homeassistant
    image: ghcr.io/home-assistant/home-assistant:stable
    volumes:
    - ${APP_DATA_DIR}/config:/config
    restart: unless-stopped
    privileged: true
    network_mode: host
    labels:
      runtipi.managed: true
 
```
