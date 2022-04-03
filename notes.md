# Docker Deep Dive ACloudGuru

- notes: 7752kvxaasdasd

- what is docker?

  - docker
    - company Docker, Inc.
    - Docker the container runtime and orchestration engine
    - Docker the open source project (Moby)
  - two main editions
    - enterprise edition
    - community edition
  - quarterly releases

- commands -> CentOS7

  - remove all traces of docker

  ```bash
  sudo yum remove -y docker \
                    docker-client \
                    docker-client-latest \
                    docker-common \
                    docker-latest \
                    docker-latest-logrotate \
                    docker-logrotate \
                    docker-engine
  ```

  - install docker CE & utilities needed

  ```bash
  sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
  ```

  ```bash
  sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
  ```

  ```bash
  sudo yum -y install docker-ce
  ```

  - enable and start docker

  ```bash
  sudo systemctl start docker && sudo systemctl enable docker
  ```

  - add cloud_user to the docker group

  ```bash
  sudo usermod -aG docker cloud_user
  ```

- Dockerfile is instructions to build images
- containers
  - runnable instance of an image
  - connect a container to networks
  - attach storage
  - create a new image based on its current state
- services
  - scale containers across multiple Docker daemons
  - Docker Swarm
  - define the desired state
  - service is load-balanced
- Docker Swarm
  - multiple docker daemon
  - docker daemons communicate using Docker API
- docker engine
  - based on open standards outline by the Open Container Initiative
  - major components
    - Docker Client
    - Docker Daemon
    - containerd
    - runc
- the first release of docker
  - docker daemon
  - LXC
    - namespaces
    - control groups
    - linux-specific
- LXC was later replaced with libcontainer
  - platform agnostic
- issues with the monoloithic docker daemon
  - harder to innovate
  - slow
  - not what the ecosystem wanted
- docker became more modular
- open container initiative
- runc
  - implementation of the OCI container runtime spec
  - create containers
  - lightweight wrapper for libcontainer
- containerd
  - manage container lifecycle
    - start
    - stop
    - pause
    - delete
  - image management
    - push
    - pull
  - part of the 1.11 release
- shim
  - implementation of daemonless containers
  - containerd forks an instance of runc for each new container
  - runc process exits after the container is created
  - shim process becomes the container parent
  - responsible for
    - STDIN and STDOUT
    - reporting exit status to the docker daemon
- `docker container run -it --name <name> <image>:<tag>`
- creating a container

  - use the CLI
  - docker uses the approppiate API payload
  - posts to the correct API endpoint
  - daemon receives instructions
  - calls containerd
  - containerd creates bundle OCI
  - runc craetes a contianer using the OCI bundle

- Docker images

  - use images to create an instance of a container
  - compromised of multiple layers
  - build time constructs
  - built from the instructions
  - images are made of multiple layers
  - each layer represents an instruction from Dockerfile

  - Docker Hub

    - public Docker registry
    - provided by Docker
    - features
      - repositories
      - teams and organizations
      - official images
      - publisher images
      - builds
      - webhooks

- docker commands
  - `docker image`
    - `docker image ls`
    - `docker image pull <image-name>`
    - `docker image inspect <image-id>`
  - `docker container`
    - `docker container ls`
    - `docker container run <image-name>`
      - `docker container run busybox`
    - `docker container ps`
      - list running containers
    - `docker container inspect <container-id>`
    - `docker container top <container-id>`
    - `docker container attach <container-id>`
    - `docker container start <container-id>`
    - `docker container stop <container-id>`
    - `docker container logs <container-id>`
    - `docker container stats <container-id>`
    - `docker container exec -it <container-id> /bin/bash`
    - `docker container pause <container-id>`
    - `docker container unpause <container-id>`
    - `docker container prune`
    - `docker rm -f <container-id>`
