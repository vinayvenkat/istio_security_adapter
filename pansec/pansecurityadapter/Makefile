all: clean build_adapter docker_build3 docker_push3
.PHONY: all 

clean:
	rm -rf bin
build_adapter: 
	CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -v -o bin/pansecurityadapter ./pansecurityadapter/cmd/

docker_build:
	docker build -t vinayvenkat/pan-servicemesh-security:gcp_onprem .

docker_push:
	docker push vinayvenkat/pan-servicemesh-security:gcp_onprem

docker_build2:
	docker build -t vinayvenkat/pan-servicemesh-security:2.0 .

docker_push2:
	docker push vinayvenkat/pan-servicemesh-security:2.0

docker_build3:
	docker build -t vinayvenkat/pan-servicemesh-security:panorama_v2 .

docker_push3:
	docker push vinayvenkat/pan-servicemesh-security:panorama_v2
