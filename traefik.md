### Traefik

```sh
# Creamos el directorio que albegará las configuraciones de Traefik y el docker-compose.yml

mkdir ~/traefik && cd traefik
mkdir -p data/configurations
touch docker-compose.yml
```
El siguiente paso será editar el fichero docker-compose.yml . En él definiremos la versión del contenedor a utilizar, en este caso la etiqueta será latest . También definiremos que los puertos que expondrá serán 80  y 443 . Definiremos volúmenes y ficheros a importar dentro del contenedor (configuración estática, dinámica y fichero para letsencrypt). También usaremos una red llamada proxy  que daremos de alta más adelante para usarla con los contenedores.

Por último, una parte importante son las etiquetas con las que configuraremos este despliegue. Éstas definirán el dominio a usar ( traefik.canonigos.me ), el entrypoint  que hemos llamado websecure y que irá dirigido contra el Dashboard de Traefik ( service=api@internal ) y la red que tendrá agregada el contenedor ( proxy ).

```yaml
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
```
Una vez configurado el fichero docker-compose.yml , pasamos a editar la configuración estática de Traefik:

```yaml
# data/traefik.yml
api:
  dashboard: true
 
# Definición de los entrypoints
entryPoints:
  web:
    address: :80
    http:
      redirections:
        entryPoint:
          to: websecure
 
  websecure:
    address: :443
    http:
      middlewares:
        - secureHeaders@file
      tls:
        certResolver: letsencrypt
 
# Proveedores: Docker y configuración dinámica mediante provider de tipo file.
providers:
  docker:
    endpoint: "unix:///var/run/docker.sock"
    exposedByDefault: false
  file:
    filename: /configurations/dynamic.yml
 
# Configuración Let's Encrypt que usaremos en el entrypoint websecure
certificatesResolvers:
  letsencrypt:
    acme:
      email: admin@canonigos.me
      storage: acme.json
      keyType: EC384
      httpChallenge:
        entryPoint: web
```

```sh
# Creamos el fichero de configuración estática
touch data/traefik.yml

# Creamos el fichero de configuración dinámica.
touch data/configurations/dynamic.yml

# Creamos el fichero que albergará las respuestas de Let's Encrypt
touch data/acme.json
chmod 600 data/acme.json

```
