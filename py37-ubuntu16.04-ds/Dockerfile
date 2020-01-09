FROM ubuntu:16.04

LABEL maintainer="Kuldeep Shah <kul.n.shah@gmail.com>"

# Install lower level dependencies
RUN apt-get update --fix-missing \
  && apt-get remove -y x264 libx264-dev

RUN apt install -y software-properties-common \
    && add-apt-repository ppa:deadsnakes/ppa \
    && apt update

RUN apt-get install -y \
  apt-utils \
  bc \
  build-essential \
  cmake \
  curl \
  git \
  pkg-config \
	python-dev \
  python-pip \
  python3.7-dev \
  python3-pip \
  tree \
  unzip \
  vim \
  wget \
  xclip \
  zip

RUN apt-get clean \
	&& apt-get autoremove \
	&& rm -rf /var/lib/apt/lists/*

RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.7 10 \
    && update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 10

RUN pip install --upgrade pip

RUN python3 -m pip --no-cache-dir install \
	bokeh \
  imblearn \
  jupyter \
  numpy==1.15 \
  matplotlib \
  pandas \
	plotly \
  requests \
  scikit-learn \
	seaborn \
  tqdm

RUN echo 'export PS1="\[\033[04;35m\]\u @ \[\033[04;35m\]\H \[\033[00m\][\w] $ "' >> ~/.bashrc \
  && echo 'alias mkdir="mkdir -v"' >> ~/.bashrc \
  && echo 'alias tree="tree --dirsfirst --sort=name"' >> ~/.bashrc \
  && echo 'set -o noclobber' >> ~/.bashrc