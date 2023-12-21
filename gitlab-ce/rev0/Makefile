REGISTORY           = 
USERNAME            = y0sshi316n
DOCKERFILE          = ./Dockerfile
IMAGE               = my-gitlab-ce
TAG                 = rev0.1
CR_PAT              = ${CR_PAT_GITHUB_PACKAGE}
PORT_SSH            = 31022
PORT_HTTP           = 31080
PORT_HTTPS          = 31443
GITLAB_EXTERNAL_URL = https:\\/\\/gitlab.y0sshi316n.diskstation.me

build : $(DOCKERFILE)
	docker image build ./ -t $(REGISTORY)$(USERNAME)/$(IMAGE):$(TAG) \
		--build-arg PORT_SSH=$(PORT_SSH)     \
		--build-arg PORT_HTTP=$(PORT_HTTP)   \
		--build-arg PORT_HTTPS=$(PORT_HTTPS) \
		--build-arg GITLAB_EXTERNAL_URL=$(GITLAB_EXTERNAL_URL)

push : login
	docker push $(REGISTORY)$(USERNAME)/$(IMAGE):$(TAG)

login :
	$(CR_PAT) | docker login $(REGISTORY) -u $(USERNAME) --password-stdin

run :
	docker run -it --rm -p $(PORT_SSH):$(PORT_SSH) -p $(PORT_HTTP):$(PORT_HTTP) -p $(PORT_HTTPS):$(PORT_HTTPS) $(REGISTORY)$(USERNAME)/$(IMAGE):$(TAG)

clean :
	docker image rm $(REGISTORY)$(USERNAME)/$(IMAGE):$(TAG)

