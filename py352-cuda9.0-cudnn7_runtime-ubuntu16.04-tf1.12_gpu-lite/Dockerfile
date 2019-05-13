FROM nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04

MAINTAINER Kuldeep Shah <Kul.n.shah@gmail.com>

# Some common environmenta variables that Python uses
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8

# Install lower level dependencies
RUN apt-get update --fix-missing

RUN apt-get install -y \
	python-dev build-essential \
	curl python3 python3-pip \
	cmake \
	curl \
	#g++ \
	#gfortran \
	git \
	#libffi-dev \
	#libfreetype6-dev \
	#libhdf5-dev \
	#libjpeg-dev \
	#liblcms2-dev \
	libopenblas-dev \
	#liblapack-dev \
	#libopenjpeg5 \
	#libpng12-dev \
	#libssl-dev \
	#libtiff5-dev \
	#libwebp-dev \
	#libzmq3-dev \
	#libglib2.0-0 \
	#libsm6 \
	#libxext6 \
	#libxrender1 \
	#libfontconfig1 \
	#nano \
	#pkg-config \
	#unzip \
	vim \
	wget \
	#libx264-dev \
	python-numpy \
	python-scipy \
	#python-nose \
	python-h5py \
	python-skimage \
	python-matplotlib \
	python-pandas \
	#python-sympy \
	python-sklearn 
	

RUN apt-get clean
RUN apt-get autoremove
RUN rm -rf /var/lib/apt/lists/*
	
# Link BLAS library to use OpenBLAS using the alternatives mechanism (https://www.scipy.org/scipylib/building/linux.html#debian-ubuntu)
RUN update-alternatives --set libblas.so.3 /usr/lib/openblas-base/libblas.so.3

RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 10
RUN update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 10

RUN pip install --upgrade pip
RUN python3 -m pip --no-cache-dir install \
	easydict \
	Cython \
	Pillow \
	#pygments \
	#six \
	#sphinx \
	#wheel \
	#zmq \
	opencv-python

# Install a specific version of TensorFlow
# You may also install anything else from pip like this
RUN python3 -m pip --no-cache-dir install \
	tensorflow-gpu==1.12.0 \
	tensorlayer
