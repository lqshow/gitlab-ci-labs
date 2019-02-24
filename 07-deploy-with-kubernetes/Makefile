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

ifneq ($(APP_NAME),)
	APP_NAME ?= $(APP_NAME)
else
	APP_NAME ?= $(IMAGE_NAME)
endif

# Where to push the docker image.
REGISTRY ?= docker.io/lqshow
IMAGE := $(REGISTRY)/$(IMAGE_NAME)
BUILD_IMAGE ?= node:latest
K8S_DIR := $(shell cd ./deploy/kubernetes && pwd)

all: install-deps test

version:
	@echo $(VERSION)

install-deps:
	@npm install --unsafe-perm

test:
	@npm test

prepare:
	@sed -ie "s/_IMAGE_NAME_/$(IMAGE_NAME)/g" $(K8S_DIR)/deployment.ci.tpl.yml
	@sed -ie "s/_IMAGE_VERSION_/$(VERSION)/g" $(K8S_DIR)/deployment.ci.tpl.yml
	@sed -ie "s/_APP_NAME_/$(APP_NAME)/g" $(K8S_DIR)/deployment.ci.tpl.yml

build:
	@docker build -t $(IMAGE):$(VERSION) -f Dockerfile --build-arg VERSION=$(VERSION) --build-arg BUILD_IMAGE=$(BUILD_IMAGE) .

publish:
	@docker push $(IMAGE):$(VERSION)

release: build publish

deploy-to-staging: prepare
	@kubectl apply -f $(K8S_DIR)/deployment.ci.tpl.yml --record

.PHONY: version \
		install-deps \
		test \
		prepare \
		build \
		publish \
		release \
		deploy-to-staging
