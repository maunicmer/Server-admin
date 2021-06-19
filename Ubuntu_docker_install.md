## Docker Install on Ubutnu 20.04

###### Software update and repository key installation
```sh
sudo apt update
sudo apt -y install apt-transport-https ca-certificates curl gnupg2 software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

## Add Docker repository

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu  $(lsb_release -cs)  stable"

## Install Docker-ce and Docker-compose 

sudo apt-get install docker-ce

## Add users to Docker gorup

sudo usermod -aG docker ${USER}

## Enable Docker Service 

sudo systemctl start docker
sudo systemctl enable docker
sudo systemctl status docker


```

