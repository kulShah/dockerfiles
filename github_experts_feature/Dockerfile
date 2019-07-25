FROM ubuntu:16.04

LABEL maintainer="Kuldeep Shah <Kul.n.shah@gmail.com>"

# Some common environmenta variables that Python uses
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8

# Install lower level dependencies
RUN apt-get update --fix-missing

RUN apt-get install -y \
  build-essential \
  cmake \
  curl \
  g++ \
  git \
  pkg-config \
  python-dev \
  python-pip \
  python3-dev \
  python3-pip \
  tree \
  vim \
  wget

RUN apt-get clean \
	&& apt-get autoremove \
	&& rm -rf /var/lib/apt/lists/*

RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 10 \
	  && update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 10 \
	  && pip install --upgrade pip

RUN python3 -m pip --no-cache-dir install \
    gitpython

RUN echo 'export PS1="\[\033[04;35m\]\u @ \[\033[04;35m\]\H \[\033[00m\][\w] $ "' >> ~/.bashrc \
    && echo 'alias mkdir="mkdir -v"' >> ~/.bashrc \
    && echo 'set -o noclobber' >> ~/.bashrc \
    && cd /home \
    && git clone https://github.com/golang/go.git \
    && export GIT_REPO_PATH='/home/go' \
    && mkdir /home/app \
    && cd /home/app/ \
    && git clone https://github.com/kulShah/github_experts_feature.git \
    && cd github_experts_feature/ \
    && python3 setup.py install