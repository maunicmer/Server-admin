### Setup Docker Swarm

1) On master node

``` sh
docker swarm init --advertise-addr "192.168.98.249"

```

2) On each worker node

``` sh
docker swarm join --token SWMTKN-1-2bhf8q1ykxsmuz442cqdpnf0rjz6ow3khwgazrc29hpnkw36xi-8b73xy6iq7n9dr8vnu1f61mua 192.168.98.249:xxxx

```
