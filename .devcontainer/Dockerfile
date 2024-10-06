FROM ubuntu:24.04

ARG BOOST_VERSION=1.86.0
ARG CMAKE_VERSION=3.30.4
ARG NUM_JOBS=8

ENV DEBIAN_FRONTEND=noninteractive

# Install package dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        build-essential \
        software-properties-common \
        autoconf \
        automake \
        libtool \
        pkg-config \
        ca-certificates \
        libssl-dev \
        wget \
        git \
        curl \
        language-pack-en \
        locales \
        locales-all \
        vim \
        gdb \
        valgrind && \
    apt-get clean

# System locale
ENV LC_ALL=id_ID.UTF-8
ENV LANG=id_ID.UTF-8
ENV LANGUAGE=id_ID.UTF-8

# Install CMake
RUN cd /tmp && \
    wget https://github.com/Kitware/CMake/releases/download/v${CMAKE_VERSION}/cmake-${CMAKE_VERSION}.tar.gz && \
    tar xzf cmake-${CMAKE_VERSION}.tar.gz && \
    cd cmake-${CMAKE_VERSION} && \
    ./bootstrap && \
    make -j${NUM_JOBS} && \
    make install && \
    rm -rf /tmp/*

# Install Boost
RUN cd /tmp && \
    BOOST_VERSION_MOD=$(echo $BOOST_VERSION | tr . _) && \
    wget https://boostorg.jfrog.io/artifactory/main/release/${BOOST_VERSION}/source/boost_${BOOST_VERSION_MOD}.tar.bz2 && \
    tar --bzip2 -xf boost_${BOOST_VERSION_MOD}.tar.bz2 && \
    cd boost_${BOOST_VERSION_MOD} && \
    ./bootstrap.sh --prefix=/usr/local && \
    ./b2 install && \
    rm -rf /tmp/*

RUN apt-get update && apt-get install -y unzip tar zip && rm -rf /var/lib/apt/lists/* 
WORKDIR /tmp
RUN git clone https://github.com/microsoft/vcpkg.git
WORKDIR /tmp/vcpkg
RUN ./bootstrap-vcpkg.sh
RUN export VCPKG_ROOT=/tmp/vcpkg
RUN export PATH=$VCPKG_ROOT:$PATH
# RUN vcpkg install nlohmann-json