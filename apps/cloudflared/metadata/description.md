
# JSON
```json
{
  "services": [
    {
      "name": "cloudflared",
      "image": "wisdomsky/cloudflared-web:2024.10.0",
      "isMain": true,
      "networkMode": "host",
      "volumes": [
        {
          "hostPath": "${APP_DATA_DIR}/data/cloudflared/config",
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
  cloudflared:
    image: wisdomsky/cloudflared-web:2024.10.0
    container_name: cloudflared
    restart: unless-stopped
    network_mode: host
    volumes:
    - ${APP_DATA_DIR}/data/cloudflared/config:/config
    labels:
      runtipi.managed: true
 
```
