FROM       ubuntu:focal
MAINTAINER y0sshi316n
LABEL      description="my ubuntu:focal image"

## timezone settings
ENV TZ Asia/Tokyo
RUN ln -snf /usr/share/zoneinfo/${TZ} /etc/localtime && echo ${TZ} > /etc/timezone

## apt nowarning
ENV DEBCONF_NOWARNINGS yes

## bashrc settings
RUN echo "\n# User Settings"                                                                                           >> /root/.bashrc
RUN echo "export PS1=\"\[\e[1;4;32m\]\u@\h:\[\e[m\]\[\e[1;33m\]\w\[\e[m\] \[\e[1;36m\][\D{%Y/%m/%d(%a)}]\[\e[m\]\n \"" >> /root/.bashrc
RUN echo "alias x=exit"                                                                                                >> /root/.bashrc
RUN echo "\n# User Settings"                                                                                           >> /etc/skel/.bashrc
RUN echo "export PS1=\"\[\e[1;4;32m\]\u@\h:\[\e[m\]\[\e[1;33m\]\w\[\e[m\] \[\e[1;36m\][\D{%Y/%m/%d(%a)}]\[\e[m\]\n \"" >> /etc/skel/.bashrc
RUN echo "alias x=exit"                                                                                                >> /etc/skel/.bashrc

## install basic packages
RUN apt-get update -y
RUN apt-get install -y \
			man-db           \
		  ssh              \
		  openssh-server   \
		  ca-certificates  \
		  rsync            \
		  neovim           \
			xclip            \
		  sudo             \
		  curl             \
		  screen           \
		  cmake            \
		  git              \
		  finger
RUN apt-get install -y \
		  build-essential  \
		  libboost-all-dev
RUN apt-get install -y \
		  python3-dev      \
		  python3-pip      \
		  python2-dev      \
		  ruby-dev         \
		  nodejs           \
		  npm              \
		  golang           \
		  default-jre      \
		  default-jdk
RUN pip3 install    \
		  numpy         \
		  opencv-python \
		  matplotlib    \
		  torch
RUN gem install neovim
RUN npm install -g neovim

## install gitlab-ce
RUN curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash
#RUN sudo EXTERNAL_URL="https://gitlab.y0sshi316n.com" apt-get install gitlab-ce
RUN sudo apt-get install -y gitlab-ce
#RUN cat /etc/gitlab/initial_root_password

## skel files
COPY ./nvim /root/.config/nvim

CMD ["/bin/bash"]

