# HomeSwarmContainers
My Docker-Compose files for setting up my docker services on my home swarm cluster. I run keepalived on all my manager nodes advertising a virtual ip on my local lan. 
Some containers use the macvlan_swarm network. 
   * HomeAssistant
   * Unifi
   * PiHole

I have these set up as such:
`docker network create --config-only --subnet 10.100.0.0/23 --gateway 10.100.0.1 -o parent=ens18 --ip-range 10.100.1.128/27 mv-config` 
`docker network create --config-only --subnet 10.100.0.0/23 --gateway 10.100.0.1 -o parent=ens18 --ip-range 10.100.1.160/27 mv-config` 
`docker network create --config-only --subnet 10.100.0.0/23 --gateway 10.100.0.1 -o parent=ens18 --ip-range 10.100.1.192/27 mv-config` 

Then create the swarm network:
`docker network create -d macvlan --scope swarm --config-from mv-config macvlan_swarm`
Note that I use a different range on each node the ensures they do not overapp ip assignments. 

# Current Services
  * HomeAssistant
  * Traefik 2.0
  * Plex
  * Jenkins
  * Portainer
  * Unifi Controller
  * Sonarr
  * Radarr
  * Deluge
  * Factorio
  * Postgres Database for HomeAssistant
  * Mongodb for Unifi
  * ELK Stack (Elasticsearch, LogStash, Kibana)
  * PiHole (WIP)
  * Test (A test service just to practice commands/mounts/etc before deploying)

## Setup
Any occurances of `${VAR}` in the compose file is set using Environmental Variables set via a .env file at the root of the repository. Docker Stack does not look for the .env file, you must either source the file or put them in your .bashrc : `export VAR="example-value"`. 

The easiest way to handle setting the environment variables inside the containers is to reference the .env file with `env_file: /path/to/.env` in your docker-compose.yaml file. 

## Docker Swarm
Get repo:
  `git clone https://github.com/dsyorkd/HomeSwarmContainers.git` 
  `cd HomeSwarmContainers`
  Run the service:
  `docker stack deploy -c docker-compose.yaml servicename`

