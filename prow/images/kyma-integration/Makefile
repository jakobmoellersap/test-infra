# Environment variables expected by this makefile
# DOCKER_TAG - dynamic tag for image
# DOCKER_PUSH_REPOSITORY - URL set by prow preset
# DOCKER_PUSH_DIRECTORY

IMG_NAME = kyma-integration
IMG = $(DOCKER_PUSH_REPOSITORY)$(DOCKER_PUSH_DIRECTORY)/$(IMG_NAME)

# build and push on PR, tag with PR number
.PHONY: ci-pr
ci-pr: build-image push-image


# build and push on postsubmit, tag with commit ID and current
.PHONY: ci-release
ci-release: build-image tag-image push-image


# tag image with current tag
.PHONY: tag-image
tag-image:
	@echo "___ Tagging as current ___"
	docker tag $(IMG):$(DOCKER_TAG) $(IMG):current
ifdef DOCKER_POST_PR_TAG
	@echo "___ Tagging with pr number ___"
	docker tag $(IMG):$(DOCKER_TAG) $(IMG):$(DOCKER_POST_PR_TAG)
endif


# build image and tag it with commit ID or PR number
.PHONY: build-image
build-image:
	docker build -t $(IMG):$(DOCKER_TAG) --build-arg K8S_VERSION=1.18 --build-arg commit=$(DOCKER_TAG) .


# push image with all tags
.PHONY:push-image
push-image:
	@echo "___ Pushing image with all tags ___"
	docker push -a $(IMG)
