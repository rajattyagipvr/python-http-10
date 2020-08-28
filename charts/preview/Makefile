OS := $(shell uname)

## rajat added helm delete as jx step helm uses a custom helm which is templating and causing issues in helm   upgrade
HELM_RELEASE_NAME := $(shell helm list -n jx-rajattyagipvr-$(APP_NAME)-pr-$(PULL_NUMBER) -q)

preview:
ifeq ($(OS),Darwin)
	sed -i "" -e "s/version:.*/version: $(PREVIEW_VERSION)/" Chart.yaml
	sed -i "" -e "s/version:.*/version: $(PREVIEW_VERSION)/" ../*/Chart.yaml
	sed -i "" -e "s/tag:.*/tag: $(PREVIEW_VERSION)/" values.yaml
else ifeq ($(OS),Linux)
	sed -i -e "s/version:.*/version: $(PREVIEW_VERSION)/" Chart.yaml
	sed -i -e "s/version:.*/version: $(PREVIEW_VERSION)/" ../*/Chart.yaml
	sed -i -e "s|repository:.*|repository: $(DOCKER_REGISTRY)\/702769831180\/python-http-10|" values.yaml
	sed -i -e "s/tag:.*/tag: $(PREVIEW_VERSION)/" values.yaml
else
	echo "platfrom $(OS) not supported to release from"
	exit -1
endif
	echo "  version: $(PREVIEW_VERSION)" >> requirements.yaml

## added rajat
ifndef HELM_RELEASE_NAME
	@echo NO EXISTING RELEASE SO FRESH INSTALL
else
	@helm delete preview -n jx-rajattyagipvr-$(APP_NAME)-pr-$(PULL_NUMBER)
endif
	jx step helm build
