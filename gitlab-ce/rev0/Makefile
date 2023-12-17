REGISTORY  = 
USERNAME   = y0sshi316n
DOCKERFILE = ./Dockerfile
IMAGE      = my-gitlab-ce
TAG        = rev0.1
CR_PAT     = ${CR_PAT_GITHUB_PACKAGE}

build : $(DOCKERFILE)
	docker image build ./ -t $(REGISTORY)$(USERNAME)/$(IMAGE):$(TAG)

push : login
	docker push $(REGISTORY)$(USERNAME)/$(IMAGE):$(TAG)

login :
	echo $(CR_PAT) | docker login $(REGISTORY) -u $(USERNAME) --password-stdin

run :
	docker run -it --rm -p 1080:80 $(REGISTORY)$(USERNAME)/$(IMAGE):$(TAG)

clean :
	docker image rm $(REGISTORY)$(USERNAME)/$(IMAGE):$(TAG)

