FROM python:3.7
MAINTAINER Kyle Hounslow <kyle.hounslow@gmail.com>

RUN apt-get update 
RUN apt-get install -y build-essential 
RUN apt-get install -y cmake 
RUN apt-get install -y wget 
RUN apt-get install -y git 
RUN apt-get install -y unzip 
RUN apt-get install -y yasm 
RUN apt-get install -y pkg-config 
RUN apt-get install -y libjpeg-dev 
RUN apt-get install -y libtiff-dev 
RUN apt-get install -y libpng-dev 
RUN apt-get install -y libavcodec-dev 
RUN apt-get install -y libavformat-dev 
RUN apt-get install -y libswscale-dev 
RUN apt-get install -y libv4l-dev 
RUN apt-get install -y libatlas-base-dev 
RUN apt-get install -y gfortran 
RUN apt-get install -y libtbb2 
RUN apt-get install -y libtbb-dev 
RUN apt-get install -y libpq-dev 
RUN apt-get install -y libgtk2.0-dev 

RUN apt-get -y clean all
RUN rm -rf /var/lib/apt/lists/*

RUN pip install numpy

WORKDIR /
ARG OPENCV_VERSION=4.1.0
RUN wget https://github.com/opencv/opencv/archive/${OPENCV_VERSION}.zip \
&& unzip ${OPENCV_VERSION}.zip \
&& mkdir /opencv-${OPENCV_VERSION}/cmake_binary \
&& cd /opencv-${OPENCV_VERSION}/cmake_binary \
&& cmake -DBUILD_TIFF=ON \
  -DBUILD_opencv_java=OFF \
  -DWITH_CUDA=OFF \
  -DENABLE_AVX=ON \
  -DWITH_OPENGL=ON \
  -DWITH_OPENCL=ON \
  -DWITH_IPP=ON \
  -DWITH_TBB=ON \
  -DWITH_EIGEN=ON \
  -DWITH_V4L=ON \
  -DBUILD_TESTS=OFF \
  -DBUILD_PERF_TESTS=OFF \
  -DCMAKE_BUILD_TYPE=RELEASE \
  -DCMAKE_INSTALL_PREFIX=$(python3.7 -c "import sys; print(sys.prefix)") \
  -DPYTHON_EXECUTABLE=$(which python3.7) \
  -DPYTHON_INCLUDE_DIR=$(python3.7 -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())") \
  -DPYTHON_PACKAGES_PATH=$(python3.7 -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())") \
  -DBUILD_EXAMPLES=ON \
  .. \
&& make -j$(nproc) install

RUN rm /${OPENCV_VERSION}.zip
RUN ln -s \
  /usr/local/python/cv2/python-3.7/cv2.cpython-37m-x86_64-linux-gnu.so \
  /usr/local/lib/python3.7/site-packages/cv2.so
