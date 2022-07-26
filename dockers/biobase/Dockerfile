FROM ubuntu:20.04

RUN apt-get update && apt-get install -y \
  software-properties-common

RUN add-apt-repository -y universe &&\
  add-apt-repository -y ppa:deadsnakes/ppa &&\
  apt update

RUN apt-get update && apt-get install -y \
  build-essential \
  cmake \
  curl \
  gfortran \
  git \
  gnupg \
  libblas-dev \
  libopenblas-base \
  libopenblas-dev \
  libcurl4-openssl-dev \
  libdevil-dev \
  libffi-dev \
  libfontconfig1-dev \
  libfribidi-dev \
  libgdbm-dev \
  libgeos-dev \
  libgit2-dev \
  libharfbuzz-dev \
  libjsoncpp-dev \
  liblapack3 \
  liblapack-dev \
  liblapacke-dev \
  liblzo2-dev \
  libncurses5-dev \
  libnss3-dev \
  libreadline-dev \
  libssl-dev \
  libxml2-dev \
  locales \
  neovim \
  python3-dev \
  python3-pip \
  software-properties-common \
  tmux \
  wget \
  zlib1g-dev \
  zsh

RUN echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen \
	&& locale-gen en_US.utf8 \
	&& /usr/sbin/update-locale LANG=en_US.UTF-8

ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8

RUN cd /opt && wget ftp://ftp.gnu.org/gnu/gsl/gsl-2.7.tar.gz &&\
  tar zxvf gsl-2.7.tar.gz && cd gsl-2.7 &&\
  ./configure --prefix /usr/local && make && make install

RUN gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9 &&\
  gpg -a --export E298A3A825C0D65DFD57CBB651716619E084DAB9 | apt-key add - &&\
  add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu focal-cran40/' &&\
  apt update &&\
  apt install -y r-base r-base-core r-recommended r-base-dev

RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 20
RUN python -m pip install -U numpy
RUN python -m pip install -U scipy
RUN python -m pip install -U pandas
RUN python -m pip install -U matplotlib
RUN python -m pip install -U bx-python

RUN R --slave --vanilla -e 'install.packages(c("tidyverse", "BiocManager", "data.table", "devtools", "remotes", "R.utils"), dependencies=TRUE, repos="http://cran.rstudio.com/", lib="/usr/lib/R/library")' 
RUN R --slave --vanilla -e 'BiocManager::install("Biostrings", update=TRUE)' 
RUN R --slave --vanilla -e 'update.packages(ask=FALSE, repos="http://cran.rstudio.com/")'
