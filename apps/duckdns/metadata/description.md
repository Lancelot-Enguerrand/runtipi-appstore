
# JSON
```json
{
  "services": [
    {
      "name": "duckdns",
      "image": "lscr.io/linuxserver/duckdns:b14c520a-ls8",
      "isMain": true,
      "environment": {
        "PUID": "1000",
        "PGID": "1000",
        "TZ": "${TZ}",
        "SUBDOMAINS": "${DUCKDNS_SUBDOMAINS}",
        "TOKEN": "${DUCKDNS_TOKEN}",
        "UPDATE_IP": "${DUCKDNS_UPDATE_IP}",
        "LOG_FILE": "${DUCKDNS_ENABLE_LOG_FILE}"
      },
      "volumes": [
        {
          "hostPath": "${APP_DATA_DIR}/data/config",
          "containerPath": "/config"
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
  duckdns:
    container_name: duckdns
    image: lscr.io/linuxserver/duckdns:b14c520a-ls8
    environment:
    - PUID=1000
    - PGID=1000
    - TZ=${TZ}
    - SUBDOMAINS=${DUCKDNS_SUBDOMAINS}
    - TOKEN=${DUCKDNS_TOKEN}
    - UPDATE_IP=${DUCKDNS_UPDATE_IP}
    - LOG_FILE=${DUCKDNS_ENABLE_LOG_FILE}
    volumes:
    - ${APP_DATA_DIR}/data/config:/config
    restart: unless-stopped
    networks:
    - tipi_main_network
    labels:
      runtipi.managed: true
 
```
