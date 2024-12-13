
# JSON
```json
{
  "services": [
    {
      "name": "mdns-repeater",
      "image": "angelnu/mdns_repeater:latest",
      "isMain": true,
      "networkMode": "host",
      "environment": {
        "hostNIC": "${MDNS_HOST_NIC}",
        "dockerNIC": "${MDNS_DOCKER_NIC}"
      }
    }
  ]
} 
```
# YAML
```yaml
services:
  mdns-repeater:
    image: angelnu/mdns_repeater:latest
    container_name: mdns-repeater
    restart: unless-stopped
    network_mode: host
    environment:
    - hostNIC=${MDNS_HOST_NIC}
    - dockerNIC=${MDNS_DOCKER_NIC}
    labels:
      traefik.enable: false
      runtipi.managed: true
 
```
