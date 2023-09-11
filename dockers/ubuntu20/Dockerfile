FROM rocker/r-ver:4.2.3
LABEL org.opencontainers.image.authors="viktor.s.petuhov@ya.ru"


RUN apt-get update --yes && apt-get install --no-install-recommends --yes \
  build-essential \
  cmake \
  git \
  less \
  libbamtools-dev \
  libboost-dev \
  libboost-iostreams-dev \
  libboost-log-dev \
  libboost-system-dev \
  libboost-test-dev \
  libcurl4-openssl-dev \
  libssl-dev \
  libz-dev \
  nano \
  pandoc

RUN apt-get install --yes tcl-dev tk-dev libpng-dev libxml2-dev libharfbuzz-dev libfribidi-dev libfreetype6-dev libpng-dev libtiff5-dev libjpeg-dev

RUN mkdir /project
WORKDIR "/project"

RUN \
  git clone https://github.com/potulabe/dropEst.git
#    \
#   mkdir -p ~/R/x86_64-redhat-linux-gnu-library/4.2.3

# RUN \
#   R -e 'chooseCRANmirror(ind=52); install.packages(c("rlang", "httr", "gargle", "vctrs", "devtools"), dependencies = TRUE)'

# RUN echo R_LIBS_USER=/home/user/R/x86_64-redhat-linux-gnu-library/4.2.3 > .Renviron

RUN \
  R -e 'chooseCRANmirror(ind=69); install.packages(c("devtools", "Rcpp","RcppEigen", "RInside", "Matrix", "optparse", "rmarkdown", "withr", "misc3d"), dependencies = TRUE); install.packages("ks", dependencies=c("Depends", "Imports", "LinkingTo")); devtools::install_local("dropEst/dropestr/", dependencies=T)'

RUN \
  mkdir -p dropEst/build && \
  cd dropEst/build && \
  cmake ../ && \
  make

ENV PATH=/project/dropEst/build:$PATH \
  LD_LIBRARY_PATH=/usr/local/lib/R/lib/:$LD_LIBRARY_PATH \
  R_PROFILE=~/.Rprofile

ENTRYPOINT ["/bin/bash"]

# dependencies ‘pkgdown’, ‘roxygen2’, ‘rversions’, ‘urlchecker’ are not available for package ‘devtools’
# R -e 'chooseCRANmirror(ind=69); install.packages(c("pkgdown", "roxygen2", "rversions", "urlchecker"), dependencies=TRUE)'