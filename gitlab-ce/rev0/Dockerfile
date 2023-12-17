FROM       ubuntu:focal
MAINTAINER y0sshi316n
LABEL      description="my gitlab-ce image"

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
			&& apt-get update -y
RUN apt-get install -y software-properties-common \
			&& add-apt-repository ppa:apt-fast/stable \
			&& apt-get update -y \
			&& apt-get -y install apt-fast
RUN apt-fast clean
RUN apt-fast install -y \
			curl \
			openssh-server \
			openssh-client \
			ca-certificates \
			tzdata \
			cron \
			perl \
			net-tools

## install gitlab-ce
ARG GITLAB_EXTERNAL_URL=y0sshi316n
RUN curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | bash
RUN apt-fast install -y gitlab-ce
RUN sed -i -e "s/\(^external_url 'http:\/\/gitlab.\)example\(.com'\)/\1${GITLAB_EXTERNAL_URL}\2/g" /etc/gitlab/gitlab.rb
RUN sed -i -e "s/# \(package\['detect_init'\] = \)true/\1false/g" /etc/gitlab/gitlab.rb
RUN echo "/opt/gitlab/embedded/bin/runsvdir-start &" >> /etc/rc.local
RUN echo "/usr/bin/gitlab-ctl reconfigure" >> /etc/rc.local
RUN echo "/usr/bin/gitlab-ctl restart" >> /etc/rc.local

CMD ["/bin/bash"]

