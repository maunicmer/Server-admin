```sh
# Agregamos repositorio de docker para Debian y actualizamos fuentes.
sudo add-apt-repository \
"deb [arch=amd64] https://download.docker.com/linux/debian \
$(lsb_release -cs) \
stable"
sudo apt update
 
# Instalamos docker-ce y agregamos nuestro usuario al grupo docker.
# De esta forma no necesitaremos ejecutar el comando docker mediante sudo.
sudo apt install docker-ce
sudo usermod -aG docker ${USER}
 
# Podemos comprobar tras cerrar sesión y volver a entrar que ya tenemos docker instalado.
admin@ip-10-0-3-96:~$ docker ps
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
 
# Descargamos docker-compose y lo hacemos ejecutable
sudo curl -L https://github.com/docker/compose/releases/download/1.27.4/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
 
# Comprobamos la versión instalada
admin@ip-10-0-3-96:~$ docker-compose --version
docker-compose version 1.27.4, build 40524192
```
