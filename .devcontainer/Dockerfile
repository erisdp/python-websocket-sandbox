# Update the VARIANT arg in docker-compose.yml to pick a Node version: 10, 12, 14 and Java version: 11, 12, 13, 14
FROM ubuntu:latest

ARG PYTHON_VARIANT=3.6.5

ENV DEBIAN_FRONTEND=noninteractive

# user settings
ARG USER_UID=1000
ARG USER_GID=$USER_UID
ARG DOCKER_USER=docker
ARG DOCKER_PASSWORD=docker
ENV HOME /home/docker

RUN echo "root:root" | chpasswd
RUN useradd -m --uid ${USER_UID} --groups sudo ${DOCKER_USER} \
    && echo ${DOCKER_USER}:${DOCKER_PASSWORD} | chpasswd

# alias settings
RUN echo '' >> $HOME/.bashrc
RUN echo '# add local' >> $HOME/.bashrc
RUN echo 'alias ls="ls --color=auto"' >> $HOME/.bashrc

# locale settings
RUN apt-get update

RUN apt-get -y install --no-install-recommends locales \
    && locale-gen ja_JP.UTF-8
RUN echo '' >> $HOME/.bashrc
RUN echo "export LANG=ja_JP.UTF-8" >> $HOME/.bashrc

# tools settings
RUN apt-get -y install --no-install-recommends ca-certificates
RUN apt-get -y install --no-install-recommends zip unzip curl wget openssl git vim

# python settings
# RUN apt-get install -y python3 python3-pip
RUN apt-get -y install --no-install-recommends build-essential checkinstall
RUN apt-get -y install --no-install-recommends libbz2-dev libdb-dev libreadline-dev \
                                               libffi-dev libgdbm-dev liblzma-dev  \
                                               libncursesw5-dev libsqlite3-dev libssl-dev \
                                               zlib1g-dev uuid-dev tk-dev
RUN git clone https://github.com/pyenv/pyenv.git $HOME/.pyenv

RUN echo '' >> $HOME/.bashrc
RUN echo 'export PATH=$HOME/.pyenv/bin:$PATH' >> $HOME/.bashrc
RUN echo 'eval "$(pyenv init -)"' >> ~/.bashrc

ENV PATH $HOME/.pyenv/bin:$PATH
RUN eval "$(pyenv init -)"
RUN pyenv install -v ${PYTHON_VARIANT} && \
    pyenv global ${PYTHON_VARIANT}
RUN chown -R docker:docker $HOME/.pyenv

# awscli settings
RUN wget https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip
RUN unzip awscli-exe-linux-x86_64.zip && ./aws/install
RUN rm -f awscli-exe-linux-x86_64.zip

RUN apt-get -y autoremove -y
RUN apt-get -y clean
RUN rm -rf /var/lib/apt/lists/*

ENV DEBIAN_FRONTEND=dialog
