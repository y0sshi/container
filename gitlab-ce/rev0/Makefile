REGISTORY         = 
USERNAME          = y0sshi316n
DOCKERFILE        = ./Dockerfile
IMAGE             = my-gitlab-ce
TAG               = rev0.1
CR_PAT            = ${CR_PAT_GITHUB_PACKAGE}
PORT_GITLAB_SSH   = 30022:30022
PORT_GITLAB_HTTP  = 30080:30080
PORT_GITLAB_HTTPS = 30443:30443

build : $(DOCKERFILE)
	docker image build ./ -t $(REGISTORY)$(USERNAME)/$(IMAGE):$(TAG)

push : login
	docker push $(REGISTORY)$(USERNAME)/$(IMAGE):$(TAG)

login :
	$(CR_PAT) | docker login $(REGISTORY) -u $(USERNAME) --password-stdin

run :
	docker run -it --rm -p $(PORT_GITLAB_SSH) -p $(PORT_GITLAB_HTTP) -p $(PORT_GITLAB_HTTPS) $(REGISTORY)$(USERNAME)/$(IMAGE):$(TAG)

clean :
	docker image rm $(REGISTORY)$(USERNAME)/$(IMAGE):$(TAG)

