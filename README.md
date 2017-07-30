
# Gitlab-on-compose
A painless way to setup gitlab and gitlab docker runners using Docker Compose.

# Usage
- `docker-compose up`
- usual gitlab setup for project at endpoint http://localhost:1000
- Find your runner registration token ($REGISTRATION_TOKEN) at `http://GITLAB_HOST/$PROJECT_GROUP/$PROJECT_NAME/settings/ci_cd`
- Register each runner by running the following in each of their containers: `gitlab-runner register -n -r $REGISTRATION_TOKEN -u http://gitlab --executor docker --docker-image alpine:latest --docker-network-mode gitlaboncompose_default`


## Script
The script to be run will be something like:
````
REGISTRATION_TOKEN=FILL_ME_IN
docker exec -it gitlaboncompose_gitlab-runner1_1 gitlab-runner register -n -r $REGISTRATION_TOKEN -u http://gitlab --executor docker --docker-image docker:latest --docker-network-mode gitlaboncompose_default --docker-privileged 
docker exec -it gitlaboncompose_gitlab-runner2_1 gitlab-runner register -n -r $REGISTRATION_TOKEN -u http://gitlab --executor docker --docker-image docker:latest --docker-network-mode gitlaboncompose_default --docker-privileged 
docker exec -it gitlaboncompose_gitlab-runner3_1 gitlab-runner register -n -r $REGISTRATION_TOKEN -u http://gitlab --executor docker --docker-image docker:latest --docker-network-mode gitlaboncompose_default --docker-privileged 
````

# Notes
- Current Gitlab version used is 9.4.1. To change gitlab (and runner) version, edit the VERSION variable in `.env`

# Licence
Released under the simplified BSD licence.
Copyright 2017. Jeshan G. BABOOA
