
# JSON
```json
{
  "services": [
    {
      "name": "suwayomi",
      "image": "ghcr.io/suwayomi/tachidesk:v1.1.1",
      "isMain": true,
      "internalPort": 4567,
      "environment": {
        "TZ": "${TZ}",
        "BIND_IP": "0.0.0.0"
      },
      "volumes": [
        {
          "hostPath": "${APP_DATA_DIR}/data",
          "containerPath": "/home/suwayomi/.local/share/Tachidesk"
        }
      ]
    }
  ]
} 
```
# YAML
```yaml
version: '3.9'
services:
  suwayomi:
    image: ghcr.io/suwayomi/tachidesk:v1.1.1
    container_name: suwayomi
    environment:
    - TZ=${TZ}
    - BIND_IP=0.0.0.0
    volumes:
    - ${APP_DATA_DIR}/data:/home/suwayomi/.local/share/Tachidesk
    ports:
    - ${APP_PORT}:4567
    restart: unless-stopped
    networks:
    - tipi_main_network
    labels:
      runtipi.managed: true
 
```
