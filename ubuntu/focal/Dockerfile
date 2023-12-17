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
RUN sed -i 's@archive.ubuntu.com@ftp.jaist.ac.jp/pub/Linux@g' /etc/apt/sources.list \
			&& apt-get update -y \
			&& apt-get install -y software-properties-common \
			&& apt-add-repository ppa:neovim-ppa/stable \
			&& add-apt-repository ppa:apt-fast/stable \
			&& apt-get update -y \
			&& apt-get -y install apt-fast \
			&& apt-fast clean
RUN apt-fast install -y \
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
RUN apt-fast install -y \
			build-essential  \
			libboost-all-dev
RUN apt-fast install -y \
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
			torch         \
			scikit-learn
RUN gem install neovim
RUN npm install -g neovim

## install gitlab-ce
RUN curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash
#RUN sudo EXTERNAL_URL="https://gitlab.y0sshi316n.com" apt-fast install -y gitlab-ce
RUN apt-fast install -y gitlab-ce
#RUN cat /etc/gitlab/initial_root_password

## NeoVim Settings
COPY ./nvim /root/.config/nvim
RUN nvim --headless \
			-c "autocmd User PackerComplete quitall" \
			-c "PackerSync"

CMD ["/bin/bash"]


