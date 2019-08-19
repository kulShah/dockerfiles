FROM nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04

LABEL maintainer="Kuldeep Shah <Kul.n.shah@gmail.com>"

# Some common environmenta variables that Python uses
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8

# Install lower level dependencies
RUN apt-get update \
		&& apt install -y software-properties-common \
		&& add-apt-repository ppa:deadsnakes/ppa

RUN apt-get update \
	&& apt-get install -y \
	python-dev build-essential \
	curl python3.7 python3-pip \
	cmake \
	curl \
	git \
	libopenblas-dev \
	python3-h5py \
	tree \
  vim \
	wget	

RUN apt-get clean && \
	apt-get autoremove && \
	rm -rf /var/lib/apt/lists/*
	
# Link BLAS library to use OpenBLAS using the alternatives mechanism (https://www.scipy.org/scipylib/building/linux.html#debian-ubuntu)
RUN update-alternatives --set libblas.so.3 /usr/lib/openblas-base/libblas.so.3

RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.7 10 && \
	update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 10 && \
	pip install --upgrade pip

RUN python3 -m pip --no-cache-dir install \
	Cython \
	easydict \
	matplotlib \
	numpy==1.16.4 \
	opencv-python \
	pandas \
	Pillow==6.0.0 \
	scipy \
	scikit-image \
	scikit-learn \
	tb-nightly \
	torch \
	torchvision

RUN echo 'export PS1="\[\033[04;35m\]\u @ \[\033[04;35m\]\H \[\033[00m\][\w] $ "' >> ~/.bashrc && \
    # echo 'alias ls="ls -1as --human-readable --block-size=M --color=always --group-directories-first --sort=extension"' >> ~/.bashrc && \
    echo 'alias mkdir="mkdir -v"' >> ~/.bashrc && \
    echo 'alias tree="tree --dirsfirst --sort=name"' >> ~/.bashrc && \
    echo 'set -o noclobber' >> ~/.bashrc