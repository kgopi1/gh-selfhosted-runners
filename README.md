# gh-selfhosted-runners

Github Selfhosted Runners

This repo contains Dockerfiles for building the Github Actions Self hosted runners.

We have created 2 images one for AWS and another for Azure.

Each image contains CLI , Terraform and Ansible Tools . Any further tool can be added by updating the dockerfile.

## Building Docker image.

1. Run the buil\*.sh script for each Cloud environment

## Running the Image

docker run \
 --detach \
 --env ORGANIZATION=<repo_name> \
 --env ACCESS_TOKEN=<access_key> \
 --name <container_name> \
 kgopi1/ghactions-aws
