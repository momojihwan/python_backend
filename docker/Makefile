.DEFAULT_GOAL := help

# GPU configuration
GPU?=0,1,2,3
DOCKER=docker

# Docker configuration
ARG_IMAGE_NAME?=tritonserver/cuda_stabe:11.1
ARG_CONTAINER_NAME?=tritonserver
ARG_CONTAINER_HOSTNAME?=$(ARG_CONTAINER_NAME)

# Container user permission
ARG_PUID?=$(shell id -u)
ARG_PGID?=$(shell id -g)

# Container network preference
ARG_PORT_SSH?=32771
ARG_PORT_TENSORBOARD?=9271
ARG_PORT_CLIENTPORT1?=8000
ARG_PORT_CLIENTPORT2?=8001
ARG_PORT_CLIENTPORT3?=8002

# Container mount point
ARG_WORKSPACE_HOST?=$(realpath ../..)
ARG_WORKSPACE_CONTAINER?=/home/mozi/Workspace

_build:
	docker build --no-cache -t $(ARG_IMAGE_NAME) \
		-f Dockerfile .

run:
	$(DOCKER) run -d --restart=unless-stopped \
		--name $(ARG_CONTAINER_NAME) \
		--ipc=host \
		--shm-size=1g \
		--ulimit memlock=-1 \
		--ulimit stack=67108864 \
		-h $(ARG_CONTAINER_HOSTNAME) \
		-e PUID=$(ARG_PUID) -e PGID=$(ARG_PGID) \
		-p $(ARG_PORT_SSH):22 \
		-p $(ARG_PORT_TENSORBOARD):6006 \
		-p $(ARG_PORT_CLIENTPORT1):8000 \
		-p $(ARG_PORT_CLIENTPORT2):8001 \
		-p $(ARG_PORT_CLIENTPORT3):8002 \
		-v $(ARG_WORKSPACE_HOST):$(ARG_WORKSPACE_CONTAINER) \
		-v /DB:/DB \
		-v /etc/timezone:/etc/timezone \
		-v /etc/localtime:/etc/localtime \
		--runtime=nvidia \
		$(ARG_IMAGE_NAME)
