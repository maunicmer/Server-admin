### Traefik

```sh
# Creamos el directorio que albegará las configuraciones de Traefik y el docker-compose.yml

mkdir ~/traefik && cd traefik
mkdir -p data/configurations
touch docker-compose.yml

# docker-compose.yml
version: '3.7'
 
services:
  traefik:
    image: traefik:latest
    container_name: traefik
    restart: always
    security_opt:
      - no-new-privileges:true
    ports:
      - 80:80
      - 443:443
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - ./data/traefik.yml:/traefik.yml:ro
      - ./data/acme.json:/acme.json
      - ./data/configurations:/configurations
    networks:
      - proxy
    labels:
      - "traefik.enable=true"
      - "traefik.docker.network=proxy"
      - "traefik.http.routers.traefik-secure.entrypoints=websecure"
      - "traefik.http.routers.traefik-secure.rule=Host(`traefik.canonigos.me`)"
      - "traefik.http.routers.traefik-secure.middlewares=user-auth@file"
      - "traefik.http.routers.traefik-secure.service=api@internal"
 
networks:
  proxy:
    external: true

# Creamos el fichero de configuración estática
touch data/traefik.yml

# Creamos el fichero de configuración dinámica.
touch data/configurations/dynamic.yml

# Creamos el fichero que albergará las respuestas de Let's Encrypt
touch data/acme.json
chmod 600 data/acme.json

```
