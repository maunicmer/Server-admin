## Docker Install on Ubutnu 20.04

###### Software update and repository key installation
```sh
sudo apt update
sudo apt -y install apt-transport-https ca-certificates curl gnupg2 software-properties-common
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
```
