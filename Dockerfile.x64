FROM ubuntu:latest

ENV DEBIAN_FRONTEND=noninteractive 

RUN apt -y update && apt -y upgrade && apt -y install \
    apt-utils \
    autoconf \
    automake \
    autopoint \
    bash \
    bison \
    build-essential \
    bzip2 \
    flex \
    g++ \
    g++-multilib \
    gettext \
    git \
    gperf \
    intltool \
    iputils-ping \
    libc6-dev-i386 \
    libffi-dev \
    libgdk-pixbuf2.0-dev \
    libltdl-dev \
    libssl-dev \
    libtool-bin \
    libxml-parser-perl \
    lzip \
    make \
    nano \
    openssl \
    p7zip-full \
    patch perl \
    pkg-config \
    python \
    python-mako \
    ruby \
    scons \
    sed \
    unzip \
    wget \
    xz-utils

RUN cd /opt && \
    git clone https://github.com/mxe/mxe.git && \
    cd /opt/mxe && \
    NPROC=$(($(nproc)+4)) && \
    make --jobs=$NPROC JOBS=$NPROC MXE_TARGETS='x86_64-w64-mingw32.static' qt5; exit 0

# 
# I don't know why this modules fail to compile in qt5 package.
# But recompiling fixs them.
# These commands are explicitly putted in separate RUN command, so it is possible to
# start from intermediate state (in case of an error your time will not be wasted).
#
RUN cd /opt/mxe && \
    make MXE_TARGETS='x86_64-w64-mingw32.static' qtquickcontrols && \
    make MXE_TARGETS='x86_64-w64-mingw32.static' qtquickcontrols2 && \
    make MXE_TARGETS='x86_64-w64-mingw32.static' qtquick3d && \
    make MXE_TARGETS='x86_64-w64-mingw32.static' qttools && \
    make MXE_TARGETS='x86_64-w64-mingw32.static' qtgraphicaleffects; exit 0

#
# Add your packages here
#

RUN cd /opt/mxe && \
    ln -s /opt/mxe/usr/bin/x86_64-w64-mingw32.static-cmake /usr/bin/cmake; exit 0

ENV PATH="/opt/mxe/usr/bin:${PATH}"