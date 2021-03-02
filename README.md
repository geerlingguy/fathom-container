# Fathom Container (Built with Ansible)

[![CI](https://github.com/geerlingguy/fathom-container/actions/workflows/build.yml/badge.svg)](https://github.com/geerlingguy/fathom-container/actions/workflows/build.yml) [![Docker pulls](https://img.shields.io/docker/pulls/geerlingguy/fathom)](https://hub.docker.com/r/geerlingguy/fathom/)

This project is composed of three main parts:

  - **Ansible project**: This project is maintained on GitHub: [geerlingguy/fathom-container](https://github.com/geerlingguy/fathom-container). Please file issues, support requests, etc. against this GitHub repository.
  - **Docker Hub Image**: If you just want to use [the `geerlingguy/fathom` Docker image](https://hub.docker.com/r/geerlingguy/fathom/) in your project, you can pull it from Docker Hub.
  - **Ansible Role**: If you need a flexible Ansible role that's compatible with both traditional servers and containerized builds, check out [`geerlingguy.fathom`](https://galaxy.ansible.com/geerlingguy/fathom/) on Ansible Galaxy. (This is the Ansible role that does the bulk of the work in managing the Fathom container.)

## Versions

Currently maintained versions include:

  - `1.x.x`, `latest`: Fathom's latest stable version.

## Standalone Usage

If you want to use the `geerlingguy/fathom` image from Docker Hub, you don't need to install or use this project at all. You can quickly build a Fathom container locally with:

    docker run -d --name=fathom -p 9000:9000 geerlingguy/fathom:latest

You can also wrap up that configuration in a `Dockerfile` and/or a `docker-compose.yml` file if you want to keep things simple. For example:

    version: "3"
    
    services:
      fathom:
        image: geerlingguy/fathom:latest
        container_name: fathom
        ports:
          - "9000:9000"
        restart: always
        # See 'Fathom persistence' for instructions for volumes.
        volumes: []

Then run:

    docker-compose up -d

Now you should be able to access the Fathom interface at `http://localhost:9000/`.

### Fathom persistence

If you would like to preserve the Fathom database and configuration, mount a volume like `-v ./fathom:/opt/fathom:rw,delegated`.

Or, if using a Docker Compose file:

    services:
      fathom:
        ...
        volumes:
          - ./fathom:/opt/fathom:rw,delegated

## Management with Ansible

### Prerequisites

Before using this project to build and maintain Fathom images for Docker, you need to have the following installed:

  - [Docker Community Edition](https://docs.docker.com/engine/installation/) (for Mac, Windows, or Linux)
  - [Ansible](http://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

### Build the image

First, install Ansible role requirements:

    ansible-galaxy install -r requirements.yml

Then, make sure Docker is running, and run the playbook to build the container:

    ansible-playbook main.yml

Once the image is built, you can run `docker images` to see the `fathom` image that was generated.

> Note: If you get an error like `Failed to import docker`, run `pip install docker`.

### Push the image to Docker Hub

See the `.travis.yml` file in this repository for how it pushes all the tagged images automatically on any commit to the `master` branch.

## License

MIT / BSD

## Author Information

This container build was created in 2019 by [Jeff Geerling](https://www.jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).
