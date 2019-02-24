SHELL := /bin/bash

DOCKER_COMPOSE_DIR := $(shell cd ./deploy/docker-compose && pwd)

all: install-deps test

install-deps:
	@npm install --unsafe-perm

test:
	@npm test

deploy-to-staging:
	@docker-compose -f $(DOCKER_COMPOSE_DIR)/docker-compose.yml up

stop:
	@docker-compose -f $(DOCKER_COMPOSE_DIR)/docker-compose.yml down

.PHONY: install-deps \
		test \
		deploy-to-staging \
		stop
