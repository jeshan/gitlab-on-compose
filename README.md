
# Gitlab-on-compose
A painless way to setup gitlab and gitlab docker runners using Docker Compose.

# Usage
- `docker-compose up`
- usual gitlab setup for project at endpoint http://localhost:1000
- Find your runner registration token ($REGISTRATION_TOKEN) at `http://GITLAB_HOST/$PROJECT_GROUP/$PROJECT_NAME/settings/ci_cd`
- Register each runner by running the following in each of their containers: `gitlab-runner register -n -r $REGISTRATION_TOKEN -u http://gitlab --executor docker --docker-image docker:latest --docker-network-mode gitlaboncompose_default --docker-privileged`


## Script
The script to be run will be something like:
````
REGISTRATION_TOKEN=FILL_ME_IN
docker exec -it gitlaboncompose_gitlab-runner1_1 gitlab-runner register -n -r $REGISTRATION_TOKEN -u http://gitlab --executor docker --docker-image docker:latest --docker-network-mode gitlaboncompose_default  --docker-volumes "/var/run/docker.sock:/var/run/docker.sock"
docker exec -it gitlaboncompose_gitlab-runner2_1 gitlab-runner register -n -r $REGISTRATION_TOKEN -u http://gitlab --executor docker --docker-image docker:latest --docker-network-mode gitlaboncompose_default  --docker-volumes "/var/run/docker.sock:/var/run/docker.sock"
docker exec -it gitlaboncompose_gitlab-runner3_1 gitlab-runner register -n -r $REGISTRATION_TOKEN -u http://gitlab --executor docker --docker-image docker:latest --docker-network-mode gitlaboncompose_default  --docker-volumes "/var/run/docker.sock:/var/run/docker.sock"
````

# Gitlab Registry
To enable the Docker Registry, define the `registry_external_url` to be `'http://localhost:4567'` 
in `/etc/gitlab/gitlab.rb` in the gitlab container. 

Map the IP of the gitlab container to the `gitlab` host in your /etc/hosts file. Obtain it with the 
following command:

`docker inspect --format '{{.NetworkSettings.Networks.gitlaboncompose_default.IPAddress}}' gitlaboncompose_gitlab_1`

## Notes:
- need the explicit `gitlab` hostname in compose.
- since the gitlab container is on a bridge network, the IP may change and you'll have to modify your hosts file again. If you don't want this, you may try running gitlab with network_mode=host.

## TODOs:
- add example on how to use a real domain
- setup a secure registry

# Notes
- Current Gitlab version used is 9.4.1. To change gitlab (and runner) version, edit the VERSION variable in `.env`
- runners bound to host socket because they need to be able to at least pull images
- the runners are also given access to the host socket when registered so that they may run builds. This is a workaround for a current limitation. See [#1986](https://gitlab.com/gitlab-org/gitlab-ci-multi-runner/issues/1986).

# Licence
Released under the simplified BSD licence.
Copyright 2017. Jeshan G. BABOOA
