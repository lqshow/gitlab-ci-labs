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

# Where to push the docker image.
REGISTRY ?= docker.io/lqshow
IMAGE := $(REGISTRY)/$(IMAGE_NAME)
BUILD_IMAGE ?= node:latest

HELM_RELEASE_NAME ?=deploy-with-hlem
HELM_CHART_DIR := $(shell cd ./deploy/helm-charts/gilab-ci-k8s-nodejs && pwd)
HELM_CI_VALUES_FILE ?= $(HELM_CHART_DIR)/ci-values.yaml

all: install-deps test

version:
	@echo $(VERSION)

install-deps:
	@npm install --unsafe-perm

test:
	@npm test

clean:
	@rm -rf ./node_modules

build:
	@docker build -t $(IMAGE):$(VERSION) -f Dockerfile --build-arg VERSION=$(VERSION) --build-arg BUILD_IMAGE=$(BUILD_IMAGE) .

publish:
	@docker push $(IMAGE):$(VERSION)

release: build publish

dry-run:
	@helm install --dry-run --debug -f $(HELM_CI_VALUES_FILE) --set image.repository=$(IMAGE) --set image.tag=$(VERSION) $(HELM_CHART_DIR)

run:
	@helm install -f $(HELM_CI_VALUES_FILE) --name "$(HELM_RELEASE_NAME)" --set image.repository=$(IMAGE) --set image.tag=$(VERSION) $(HELM_CHART_DIR)

lint:
	@helm lint -f $(HELM_CI_VALUES_FILE) --set image.repository=$(IMAGE) --set image.tag=$(VERSION) $(HELM_CHART_DIR)

.PHONY: version \
		install-deps \
		test \
		clean \
		build \
		publish \
		release \
		dry-run \
		lint \
		run
