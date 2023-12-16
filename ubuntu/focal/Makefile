ID_DockerHub = y0sshi316n
DOCKERFILE   = ./Dockerfile
IMAGE        = my-ubuntu
TAG          = focal

build : $(DOCKERFILE)
	docker image build ./ -t $(ID_DockerHub)/$(IMAGE):$(TAG)

run :
	docker run -it --rm $(ID_DockerHub)/$(IMAGE):$(TAG)

