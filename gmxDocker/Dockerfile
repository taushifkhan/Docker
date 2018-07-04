
FROM nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04

MAINTAINER Taushif Khan <tauskhan@vub.ac.be>
# CUDA configuration
ENV CUDA_HOME=/usr/local/cuda-9.0
ENV LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-9.0/lib64:/usr/local/cuda-9.0/lib64/stubs
ENV PATH=$PATH:/usr/local/cuda-9.0/bin

# Download and install Essentials
ENV GROMACS_VERSION 2018.2

RUN apt-get update -y &&\
    apt-get install -y build-essential curl cmake libxml2-dev &&\
    curl -sL ftp://ftp.gromacs.org/pub/gromacs/gromacs-${GROMACS_VERSION}.tar.gz | tar zxfv - -C /var/tmp 

#set up installation directory
RUN mkdir /var/tmp/build
WORKDIR "./build"

# installing gromacs with cud support
RUN cmake /var/tmp/gromacs-${GROMACS_VERSION} -DCMAKE_INSTALL_PREFIX=/opt/gromacs/ -DGMX_BUILD_OWN_FFTW=ON -DREGRESSIONTEST_DOWNLOAD=ON -DGMX_GPU=ON -DCUDA_TOOLKIT_ROOT_DIR=/usr/local/cuda-9.0/

RUN make -j 20
RUN make check
RUN make install
RUN /bin/bash -c 'source /opt/gromacs/bin/GMXRC; gmx -version'

# setup /home/gmx/ as work directory
RUN mkdir /home/gmx/
WORKDIR "/home/gmx/"
CMD echo "Welcome to Gromacs ${GROMACS_VERSION}"
