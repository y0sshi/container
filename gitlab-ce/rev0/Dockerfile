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
RUN apt-get install -y --no-install-recommends software-properties-common \
			&& add-apt-repository ppa:apt-fast/stable \
			&& apt-get update -y \
			&& apt-get install -y --no-install-recommends apt-fast
RUN apt-fast clean
RUN apt-fast install -y --no-install-recommends \
			vim \
			curl \
			openssh-server \
			openssh-client \
			ca-certificates \
			tzdata \
			cron \
			perl \
			net-tools \
			netcat

## install and config gitlab-ce
ARG PORT_SSH=30022
ARG PORT_HTTP=30080
ARG PORT_HTTPS=30443
ARG GITLAB_EXTERNAL_URL=y0sshi316n.diskstation.me:30080
RUN curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | bash
RUN apt-fast install -y --no-install-recommends gitlab-ce
RUN rm -rf /var/lib/apt/lists/*
RUN sed -i -e "s/\(^external_url 'http:\/\/gitlab.\)example.com'/\1${GITLAB_EXTERNAL_URL}'/g" /etc/gitlab/gitlab.rb
RUN sed -i -e "s/^# \(package\['detect_init'\] = \)true/\1false/g" /etc/gitlab/gitlab.rb
RUN sed -i -e "s/^# \(gitlab_rails\['gitlab_shell_ssh_port'\] = \)22/\1${PORT_SSH}/" /etc/gitlab/gitlab.rb
RUN sed -i -e "s/^# \(gitlab_rails\['gitlab_shell_git_timeout'\] = 800\)/\1/" /etc/gitlab/gitlab.rb

## expose ports
EXPOSE 30022
EXPOSE 30080
EXPOSE 30443

## ssh settings
RUN mkdir -p /var/run/sshd
RUN sed -i "s/^#Port 22/Port ${PORT_SSH}/" /etc/ssh/sshd_config
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin prohibit-password/' /etc/ssh/sshd_config
COPY ./config_files/authorized_keys /root/.ssh/authorized_keys

## start-up script
RUN mkdir -p /work \
			&& echo "#!/bin/bash" >> /work/run.sh \
			&& echo "/usr/sbin/sshd -D &" >> /work/run.sh \
			&& echo "/opt/gitlab/embedded/bin/runsvdir-start &" >> /work/run.sh \
			&& echo "/usr/bin/gitlab-ctl reconfigure" >> /work/run.sh \
			&& echo "/usr/bin/gitlab-ctl restart" >> /work/run.sh \
			&& echo "echo \"$ /usr/bin/gitlab-ctl reconfigure && /usr/bin/gitlab-ctl restart -> done!!\"" >> /work/run.sh \
			&& echo "tail -f /dev/null" >> /work/run.sh \
			&& chmod +x /work/run.sh

WORKDIR /work
CMD ["./run.sh"]

