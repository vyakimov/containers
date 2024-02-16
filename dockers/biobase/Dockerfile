FROM ubuntu:22.04

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Etc/UTC
RUN apt-get update && apt-get install -y tzdata

RUN apt-get update &&\
  apt-get install -y build-essential \
  cmake \
  libmariadb-dev \
  curl \
  g++ \
  gcc \
  gfortran \
  git \
  gnupg \
  libblas-dev \
  libbz2-dev \
  libcairo2-dev \ 
  libcurl4-openssl-dev \
  libdevil-dev \
  libffi-dev \
  libfontconfig1-dev \
  libfribidi-dev \
  libgdbm-dev \
  libgeos-dev \
  libgit2-dev \
  libharfbuzz-dev \
  libjpeg-dev \
  libjsoncpp-dev \
  liblapack-dev \
  liblapack3 \
  liblzma-dev \
  liblzo2-dev \
  libncurses5-dev \
  libnss3-dev \
  libopenblas-base \
  libopenblas-dev \
  libpcre2-dev \
  libpng-dev \
  libreadline-dev \
  libssh-dev \
  libssl-dev \
  libtiff5 \
  libx11-dev \
  libxml2 \
  libxml2-dev \
  libxt-dev \ 
  libzstd-dev \ 
  locales \
  make \
  neovim \
  openjdk-11-jdk \
  python3-dev \
  python3-pip \
  screen \ 
  software-properties-common \
  swig \
  texinfo \
  texlive \
  texlive-fonts-extra \
  tmux \
  wget \
  xvfb \
  zlib1g-dev \
  zsh

RUN echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen \
	&& locale-gen en_US.utf8 \
	&& /usr/sbin/update-locale LANG=en_US.UTF-8
ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8

RUN cd /usr/local/src && wget ftp://ftp.gnu.org/gnu/gsl/gsl-2.7.tar.gz &&\
  tar zxvf gsl-2.7.tar.gz && cd gsl-2.7 &&\
  ./configure --prefix /usr/local && make && make install &&\
  cd .. && rm -rf gsl-2.7*

ENV  R_VERSION=4.3.2 
RUN  cd /usr/local/src &&\
  wget https://cran.rstudio.com/src/base/R-4/R-${R_VERSION}.tar.gz &&\
  tar zxvf R-$R_VERSION.tar.gz &&\
  cd R-$R_VERSION &&\
  ./configure --prefix=/usr/local --enable-R-shlib --enable-memory-profiling --with-blas --with-lapack &&\
  make && make install &&\
  cd .. && rm -rf R-$R_VERSION* 

RUN /usr/local/bin/R --slave --silent --no-save --no-restore -e 'options(warn=2); install.packages(c("tidyverse", "data.table", "dplyr", "magrittr", "devtools", "remotes", "R.utils"), repos="http://cran.rstudio.com/", lib="/usr/local/lib/R/library")' 
RUN /usr/local/bin/R --slave --silent --no-save --no-restore -e 'options(warn=2); install.packages("BiocManager", repos="http://cran.rstudio.com/", lib="/usr/local/lib/R/library"); BiocManager::install(version = "3.18"); BiocManager::install("preprocessCore", update=TRUE, lib="/usr/local/lib/R/library"); BiocManager::install("Biostrings", update=TRUE, lib="/usr/local/lib/R/library")'
RUN /usr/local/bin/R --slave --silent --no-save --no-restore -e 'options(warn=2); install.packages("languageserver", repos="http://cran.rstudio.com/", lib="/usr/local/lib/R/library"); devtools::install_github(c("ManuelHentschel/vscDebugger", "nx10/httpgd"))' 

RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 20
RUN pip3 install -U radian
RUN pip3 install -U numpy
RUN pip3 install -U scipy
RUN pip3 install -U pandas
RUN pip3 install -U matplotlib
RUN pip3 install -U bx-python
