TAG ?= "v1.0"

# Convenience makefiles.
include ../gcloud.Makefile
include ../var.Makefile

# app.Makefile provides the main targets for installing the
# application.
# It requires several APP_* variables defined as followed.
include ../app.Makefile

APP_DEPLOYER_IMAGE ?= $(REGISTRY)/pan-servicemesh-security/deployer:$(TAG)
NAME ?= pansecurityadapters 
APP_PARAMETERS ?= { \
  "name": "$(NAME)", \
  "namespace": "$(NAMESPACE)", \
  "appName": "$(APP_NAME)", \
  "securityAdapterImageName": "$(REGISTRY)/pan-servicemesh-security:$(TAG)", \
  "securityPolicyApiEndpoint": "$(SECURITY_POLICY_API_ENDPOINT)", \
  "securityPolicyApiPort": "$(SECURITY_POLICY_API_PORT)", \
}
TESTER_IMAGE ?= $(REGISTRY)/pan-servicemesh-security/tester:$(TAG)
APP_TEST_PARAMETERS ?= { \
  "imageTester": "$(TESTER_IMAGE)" \
}

# Extend the target as defined in app.Makefile to
# include real dependencies.
app/build:: .build/pan-servicemesh-security/deployer \
            .build/pan-servicemesh-security/pan-servicemesh-security
            #.build/wordpress/tester \


.build/pan-servicemesh-security: | .build
	mkdir -p "$@"

#                           apptest/deployer/* \
#                           apptest/deployer/manifest/* 
.build/pan-servicemesh-security/deployer: .build/var/APP_DEPLOYER_IMAGE \
                           .build/var/MARKETPLACE_TOOLS_TAG \
                           .build/var/REGISTRY \
                           .build/var/TAG \
                           deployer/* \
                           manifest/* \
                           schema.yaml \
                           | .build/pan-servicemesh-security
	$(call print_target, $@)
	docker build \
	    --build-arg REGISTRY="$(REGISTRY)/pan-servicemesh-security" \
	    --build-arg TAG="$(TAG)" \
	    --build-arg MARKETPLACE_TOOLS_TAG="$(MARKETPLACE_TOOLS_TAG)" \
	    --tag "$(APP_DEPLOYER_IMAGE)" \
	    -f deployer/Dockerfile \
	    .
	docker push "$(APP_DEPLOYER_IMAGE)"
	@touch "$@"


.build/wordpress/tester: .build/var/TESTER_IMAGE
	$(call print_target, $@)
	docker pull cosmintitei/bash-curl
	docker tag cosmintitei/bash-curl "$(TESTER_IMAGE)"
	docker push "$(TESTER_IMAGE)"
	@touch "$@"

# Simulate building of primary app image. Actually just copying public image to
	#docker tag gcr.io/redlock-gcp/pan-servicemesh-security "$(REGISTRY)/pan-servicemesh-security:$(TAG)"
	#docker push "$(REGISTRY)/pan-servicemesh-security:$(TAG)"
# local registry.
.build/pan-servicemesh-security/pan-servicemesh-security: .build/var/REGISTRY \
                            .build/var/TAG \
                            | .build/pan-servicemesh-security
	$(call print_target, $@)
	docker pull gcr.io/redlock-gcp/pan-servicemesh-security:3.0.0
	docker tag gcr.io/redlock-gcp/pan-servicemesh-security:3.0.0 "$(REGISTRY)/pan-servicemesh-security:3.1.0"
	docker push "$(REGISTRY)/pan-servicemesh-security:3.1.0"
	@touch "$@"

