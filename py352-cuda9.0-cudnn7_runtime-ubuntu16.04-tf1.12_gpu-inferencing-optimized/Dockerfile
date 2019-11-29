FROM nvidia/cuda:9.0-cudnn7-runtime-ubuntu16.04

LABEL maintainer="Kuldeep Shah <Kul.n.shah@gmail.com>"

# Some common environmenta variables that Python uses
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8

ARG RT_REPO=nv-tensorrt-repo-ubuntu1604-cuda9.0-trt5.1.5.0-ga-20190427_1-1_amd64.deb

# Install lower level dependencies
RUN apt-get update --fix-missing \
  && apt-get remove -y x264 libx264-dev

RUN apt-get install -y \
  apt-utils \
  # bc \
  build-essential \
  cmake \
  checkinstall \
  curl \
  g++ \
  # gfortran \
  git \
  libatlas-base-dev \
  libavcodec-dev \
  libavformat-dev \
  libdc1394-22-dev \
  libeigen3-dev \
  libgflags-dev \
  libgstreamer0.10-dev \
  libgstreamer-plugins-base0.10-dev \
  libhdf5-dev \
  libjasper-dev \
  libjpeg8-dev \
  libopenblas-dev \
  libpng12-dev \
  libtiff5-dev \
  libswscale-dev \
  libtbb-dev \
  libv4l-dev \
  libxine2-dev \
  pkg-config \
  python-dev \
  python-pip \
  python3-dev \
  python3-pip \
  subversion \
  swig \
  # tree \
  # unzip \
  v4l-utils \
  vim \
  wget \
  x264 \
  yasm
  # zip

RUN apt-get clean \
	&& apt-get autoremove \
	&& rm -rf /var/lib/apt/lists/*

# Link BLAS library to use OpenBLAS using the alternatives mechanism (https://www.scipy.org/scipylib/building/linux.html#debian-ubuntu)
RUN update-alternatives --set libblas.so.3 /usr/lib/openblas-base/libblas.so.3

RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 10 \
	  && update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 10 \
	  && pip install --upgrade pip

RUN python3 -m pip --no-cache-dir install \
    numpy==1.15 \
    # scipy \
    matplotlib
    # scikit-image \
    # scikit-learn

RUN cd /opt \
  && git clone --verbose https://github.com/opencv/opencv.git -b 3.4.1 \
  && cd /opt \
  && wget https://github.com/opencv/opencv_contrib/archive/3.4.1.tar.gz \
  && mkdir opencv_contrib \
  && tar -xf 3.4.1.tar.gz -C opencv_contrib --strip-components 1 \
  && cd /opt/opencv \
  && mkdir release \
  && cd release \
  && cmake -G "Unix Makefiles" -DENABLE_PRECOMPILED_HEADERS=OFF -DCMAKE_CXX_COMPILER=/usr/bin/g++ \
  CMAKE_C_COMPILER=/usr/bin/gcc -DCMAKE_BUILD_TYPE=RELEASE \
  -DCMAKE_INSTALL_PREFIX=/usr/local \
  -DWITH_TBB=ON \
  -DBUILD_NEW_PYTHON_SUPPORT=ON \
  -DWITH_V4L=ON \
  -DINSTALL_C_EXAMPLES=OFF \
  -DINSTALL_PYTHON_EXAMPLES=ON \
  -DBUILD_EXAMPLES=OFF \
  -DWITH_QT=OFF \
  -DWITH_OPENGL=ON \
  -DWITH_CUDA=ON \
  -DCUDA_GENERATION=Auto \
  -DOPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules .. \
  # && make -j"$(nproc)" \
  && make -j"$(($(nproc) + 1))" \
  && make install \
  && ldconfig \
  && cd /opt/opencv/release \
  && make clean

RUN python3 -m pip --no-cache-dir install \
  Cython \
	easydict \
	Pillow \
	tensorflow-gpu==1.12.0 \
	tensorlayer==1.11.0

#Install TensorRT dependencies
RUN apt-get update \
    && apt-get install -y libcudnn7-dev=7.6.3.30-1+cuda9.0

RUN apt-get update \
    && apt-get install -y --no-install-recommends libnvinfer5=5.1.5-1+cuda9.0 \
    && apt-get install -y --no-install-recommends libnvinfer-dev=5.1.5-1+cuda9.0

# Install TensorRT
# Download TensorRT 5.1.5.0 GA for Ubuntu 1604 and CUDA 10.0 DEB local repo packages from https://developer.nvidia.com/tensorrt.
ADD ${RT_REPO} /tmp/
RUN dpkg -i /tmp/${RT_REPO} \
    && apt-key add /var/nv-tensorrt-repo-cuda9.0-trt5.1.5.0-ga-20190427/7fa2af80.pub \
    && apt-get update \
    && apt-get install -y tensorrt \
    && apt-get install -y uff-converter-tf \
    && apt-get install -y python3-libnvinfer-dev \
    && rm /tmp/${RT_REPO}

RUN echo 'export PS1="\[\033[04;35m\]\u @ \[\033[04;35m\]\H \[\033[00m\][\w] $ "' >> ~/.bashrc \
    && echo 'alias mkdir="mkdir -v"' >> ~/.bashrc \
    && echo 'set -o noclobber' >> ~/.bashrc