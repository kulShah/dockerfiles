FROM ubuntu:16.04

LABEL maintainer="Kuldeep Shah <Kul.n.shah@gmail.com>"

# Some common environmenta variables that Python uses
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8

# Install lower level dependencies
RUN apt-get update --fix-missing

RUN apt-get install -y \
  build-essential \
  cmake \
  checkinstall \
  curl \
  g++ \
  git \
  libgtk-3-dev \
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

RUN pip3 install -U -f https://extras.wxpython.org/wxPython4/extras/linux/gtk3/ubuntu-16.04 wxPython

RUN python3 -m pip --no-cache-dir install \
    numpy==1.15 \
    matplotlib \
    PyOpenGL \ 
    PyOpenGL_accelerate \
    # scikit-image \
    # scikit-learn \
    # scipy \
    spectral

#RUN cd /opt \
#  && git clone --verbose https://github.com/opencv/opencv.git -b 3.4.1 \
#  && cd /opt \
#  && wget https://github.com/opencv/opencv_contrib/archive/3.4.1.tar.gz \
#  && mkdir opencv_contrib \
#  && tar -xf 3.4.1.tar.gz -C opencv_contrib --strip-components 1 \
#  && cd /opt/opencv \
#  && mkdir release \
#  && cd release \
#  && cmake -G "Unix Makefiles" -DENABLE_PRECOMPILED_HEADERS=OFF #-DCMAKE_CXX_COMPILER=/usr/bin/g++ \
#  CMAKE_C_COMPILER=/usr/bin/gcc -DCMAKE_BUILD_TYPE=RELEASE \
#  -DCMAKE_INSTALL_PREFIX=/usr/local \
#  -DWITH_TBB=ON \
#  -DBUILD_NEW_PYTHON_SUPPORT=ON \
#  -DWITH_V4L=ON \
#  -DINSTALL_C_EXAMPLES=OFF \
#  -DINSTALL_PYTHON_EXAMPLES=ON \
#  -DBUILD_EXAMPLES=OFF \
#  -DWITH_QT=OFF \
#  -DWITH_OPENGL=ON \
#  -DWITH_CUDA=ON \
#  -DCUDA_GENERATION=Auto \
#  -DOPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules .. \
#  # && make -j"$(nproc)" \
#  && make -j"$(($(nproc) + 1))" \
#  && make install \
#  && ldconfig \
#  && cd /opt/opencv/release \
#  && make clean

RUN echo 'export PS1="\[\033[04;35m\]\u @ \[\033[04;35m\]\H \[\033[00m\][\w] $ "' >> ~/.bashrc \
    && echo 'alias mkdir="mkdir -v"' >> ~/.bashrc \
    && echo 'set -o noclobber' >> ~/.bashrc