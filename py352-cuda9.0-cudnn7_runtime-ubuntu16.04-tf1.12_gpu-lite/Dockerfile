FROM nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04

LABEL maintainer="Kuldeep Shah <Kul.n.shah@gmail.com>"

# Some common environmenta variables that Python uses
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8

# Install lower level dependencies
RUN apt-get update --fix-missing \
  && apt-get remove -y x264 libx264-dev

RUN apt-get install -y \
  apt-utils \
  bc \
  build-essential \
  cmake \
  checkinstall \
  curl \
  g++ \
  gfortran \
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
  libnvinfer5  \
  libnvinfer-dev \
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
  tree \
  unzip \
  v4l-utils \
  vim \
  wget \
  x264 \
  yasm \
  zip

RUN apt-get clean \
	&& apt-get autoremove \
	&& rm -rf /var/lib/apt/lists/*

# Link BLAS library to use OpenBLAS using the alternatives mechanism (https://www.scipy.org/scipylib/building/linux.html#debian-ubuntu)
RUN update-alternatives --set libblas.so.3 /usr/lib/openblas-base/libblas.so.3

RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 10 \
	  && update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 10 \
	  && pip install --upgrade pip

RUN python3 -m pip --no-cache-dir install numpy==1.15 \
    && python3 -m pip --no-cache-dir install scipy matplotlib scikit-image scikit-learn

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
  && make -j"$(nproc)" \
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

RUN echo 'export PS1="\[\033[04;35m\]\u @ \[\033[04;35m\]\H \[\033[00m\][\w] $ "' >> ~/.bashrc \
    && echo 'alias mkdir="mkdir -v"' >> ~/.bashrc \
    && echo 'alias tree="tree --dirsfirst --sort=name"' >> ~/.bashrc \
    && echo 'set -o noclobber' >> ~/.bashrc