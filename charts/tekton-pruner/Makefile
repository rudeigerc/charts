REPO ?= ghcr.io/rudeigerc/charts

.PHONY: help
help: ## Display this help.
	@awk 'BEGIN {FS = ":.*##"; printf "\nUsage:\n  make \033[36m<target>\033[0m\n"} /^[a-zA-Z_0-9-]+:.*?##/ { printf "  \033[36m%-15s\033[0m %s\n", $$1, $$2 } /^##@/ { printf "\n\033[1m%s\033[0m\n", substr($$0, 5) } ' $(MAKEFILE_LIST)

##@ Development

.PHONY: lint
lint: ## Examine a chart for possible issues.
	$(HELM) lint .

.PHONY: template
template: ## Render chart templates locally and display the output.
	$(HELM) template . --output-dir ./out

##@ Build

.PHONY: package
package: ## Package a chart directory into a chart archive.
	$(HELM) package .

NAME ?= $(shell $(HELM) show chart . | grep -E '^name:' | awk '{print $$2}')
VERSION ?= $(shell $(HELM) show chart . | grep -E '^version:' | awk '{print $$2}')

.PHONY: push
push: package ## Upload a chart to a registry.
	$(HELM) push ${NAME}-${VERSION}.tgz ${REPO}


.PHONY: clean
clean: ## Remove all generated files.
	@rm -rf *.tgz

##@ Build Dependencies


## Tool Binaries
HELM ?= helm
