# Example Docker Commands

Normally, containers are purpose-built to execute a certain utility, application, or service. However, on my Windows box I like to have an interactive Linux environment as dev test environment.

This Dockerfile creates an image with minimal Ubuntu 22.04. Also provides Ansible, AWS CLI, and Terraform pre-installed.

<hr />

## Assuming Docker is installed...


### Edit the Dockerfile and authorized_keys for your purposes
Place your public key in authorized_keys if you'd like to ssh into your container

### Build Image
`docker build -t my_ubuntu2204_dev_img .`

### Create Container

`docker create -p 2222:22 --name my-ubuntu2204-dev my_ubuntu2204_dev_img`

(forward host port 2222 to the container's port 22)

### Start Container
`docker start my-ubuntu2204-dev`