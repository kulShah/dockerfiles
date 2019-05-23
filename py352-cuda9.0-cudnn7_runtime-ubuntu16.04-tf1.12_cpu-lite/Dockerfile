FROM nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04

LABEL maintainer="Kuldeep Shah <Kul.n.shah@gmail.com>"

# Some common environmenta variables that Python uses
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8

# Install lower level dependencies
RUN apt-get update --fix-missing

RUN apt-get install -y \
	python-dev build-essential \
	curl python3 python3-pip \
	cmake \
	curl \
	git \
	libopenblas-dev \
	python-h5py \
	python-matplotlib \
	python-numpy \
	python-pandas \
	python-scipy \
	python-skimage \
	python-sklearn \
	tree \
  vim \
	wget	

RUN apt-get clean && \
	apt-get autoremove && \
	rm -rf /var/lib/apt/lists/*
	
# Link BLAS library to use OpenBLAS using the alternatives mechanism (https://www.scipy.org/scipylib/building/linux.html#debian-ubuntu)
RUN update-alternatives --set libblas.so.3 /usr/lib/openblas-base/libblas.so.3

RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 10 && \
	update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 10 && \
	pip install --upgrade pip

RUN python3 -m pip --no-cache-dir install \
	Cython \
	easydict \
	opencv-python \
	Pillow \
	tensorflow==1.12.0 \
	tensorlayer

RUN echo 'export PS1="\[\033[04;35m\]\u @ \[\033[04;35m\]\H \[\033[00m\][\w] $ "' >> ~/.bashrc && \
    echo 'alias ls="ls -1as --human-readable --block-size=M --color=always --group-directories-first --sort=extension"' >> ~/.bashrc && \
    echo 'alias mkdir="mkdir -v"' >> ~/.bashrc && \
    echo 'alias tree="tree --dirsfirst --sort=name"' >> ~/.bashrc && \
    echo 'set -o noclobber' >> ~/.bashrc