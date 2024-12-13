
# JSON
```json
{
  "services": [
    {
      "name": "zerotier",
      "image": "zerotier/zerotier:1.14.2",
      "isMain": true,
      "networkMode": "host",
      "command": "${NETWORK_ID}",
      "devices": [
        "/dev/net/tun"
      ],
      "capAdd": [
        "NET_ADMIN",
        "SYS_ADMIN"
      ],
      "healthCheck": {
        "test": "true"
      }
    }
  ]
} 
```
# YAML
```yaml
version: '3.7'
services:
  zerotier:
    container_name: zerotier
    image: zerotier/zerotier:1.14.2
    restart: on-failure
    command: ${NETWORK_ID}
    cap_add:
    - NET_ADMIN
    - SYS_ADMIN
    devices:
    - /dev/net/tun
    healthcheck:
      test:
      - CMD
      - 'true'
    network_mode: host
    labels:
      runtipi.managed: true
 
```
