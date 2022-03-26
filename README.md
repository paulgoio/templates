# Templates

* CI pipeline for all custom docker images.

* This works as a template for a docker images pipeline.


### Usage

* Create a repo.

* setup a gitlab-ci.yml file with the following content:

```
include:
  - project: paulgoio/templates
    file: all.yml
```

* Review branch builds; Master brnach stging build and deployment and manual production.

* Create a Dockerfile in the root of the repo.

* Create a docker-compose.yml file for the deployment.

or

```
include:
  - project: paulgoio/templates
    file: build.yml
```

* Review branch builds; Master branch only production builds.

* Create a Dockerfile in the root of the repo.

or

```
include:
  - project: paulgoio/templates
    file: deploy.yml
```
* Production deployments without staging.

* Create a docker-compose.yml file for the deployment.


### Variables you need to set

For Building and pushing the image to repo:

* ```DOCKER_PASSWD``` : password for the Docker repo; For Dockerhub this would be an API key.

* ```DOCKER_USER``` : user used to authenticate with repo.

* ```DOCKER_REPO``` : reponame that the image gets pushed to (For dockerhub this would be the account name; test would push to test/reponame:production)

For Deployment:

* ```SSH_HOST``` : the ip or a dns name pointing to the deployment server with ssh and docker as well as docker-compose installed

* ```SSH_PORT``` : the ssh port on that deployment server

* ```SSH_USER``` : ssh user used to deploy the services

* ```SSH_PRIVATE_KEY``` : private key used to authenticate with deployment server; you can just generate a new key and copy the key with `ssh-copy-id user@server` to the server


### Variables you can set in your project

* ```DOMAIN``` : is meant for your main domain [www.example.org], is going to be set for the env

* ```STAGING_DOMAIN``` : is meant for your staging domain [www.staging.example.org], is going to be set for the env

* ```KEY1``` ```KEY2``` ```KEY3``` : any passwd or key that can be set in your docker-compose file

* ```URL1``` ```URL2``` ```URL3``` : any URL or IP that can be set in your docker-compose file

* ```IP``` : any IP address or list of IP addresse's you want to use in your project

* ```EMAIL``` : reserved for email address usage in project


### Predefined Variables you can use

* ```REPO``` : name of repository

* ```ENV``` : truncated name of the environment for dns, url or docker/k8s labels


### How it works

After every commit to master the staging tag gets build and pushed to your repo. From a staging-$Domain site is created to do a last review/testing. The server gets autostoped after 12 hours. The user can manualy retag the functional staging image to production and deploy the production server.


Every other branch will be build in the review/branchname environment; The environment/image is valid for a week.
