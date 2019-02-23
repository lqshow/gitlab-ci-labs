SHELL := /bin/bash

ifneq ($(PRODUCTION_VERSION),)
	VERSION ?= $(PRODUCTION_VERSION)
else
	VERSION ?= $(shell git rev-parse --short HEAD)
endif

ifneq ($(IMAGE_NAME),)
	IMAGE_NAME ?= $(IMAGE_NAME)
else
	IMAGE_NAME ?= app
endif

ifneq ($(CONTAINER_NAME),)
	CONTAINER_NAME ?= $(CONTAINER_NAME)
else
	CONTAINER_NAME ?= app
endif

# Where to push the docker image.
REGISTRY ?= docker.io
IMAGE := $(REGISTRY)/$(IMAGE_NAME)
BUILD_IMAGE ?= node:latest


all: install-deps test

version:
	@echo $(VERSION)

install-deps:
	@npm install --unsafe-perm

test:
	@npm test

image-build:
	@docker build -t $(IMAGE):$(VERSION) -f Dockerfile --build-arg VERSION=$(VERSION) --build-arg BUILD_IMAGE=$(BUILD_IMAGE) .

image-publish:
	@docker push $(IMAGE):$(VERSION)

deploy-to-staging:
	@docker stop $(CONTAINER_NAME) && docker rm $(CONTAINER_NAME) || true
	@docker run -d --name $(CONTAINER_NAME) \
			--restart always \
			--env NODE_ENV=test \
			-p 10010:10010 \
			$(IMAGE):$(VERSION)

.PHONY: version \
		install-deps \
		test \
		image-build \
		image-publish \
		deploy-to-staging
