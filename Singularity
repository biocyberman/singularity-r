BootStrap: docker
From: nvidia/cuda:10.2-base-ubuntu18.04

%labels
  Maintainer Jeremy Nicklas
  R_Version 3.6.3

%apprun R
  exec R "${@}"

%apprun Rscript
  exec Rscript "${@}"

%runscript
  exec R "${@}"

%post
  # Software versions
  export R_VERSION=3.6.3
  # get rid of tzdata nagging
  export DEBIAN_FRONTEND=noninteractive 

  # Get dependencies
  apt-get update
  apt-get install -y --no-install-recommends locales

  # Configure default locale
  echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen
  locale-gen en_US.utf8
  /usr/sbin/update-locale LANG=en_US.UTF-8
  export LC_ALL=en_US.UTF-8
  export LANG=en_US.UTF-8

#install R and R packages
#specific version of R to be installed
export R_BASE_VERSION=3.6.3
#add R CRAN mirror to apt sources to install R using APT
apt-key adv --keyserver keyserver.ubuntu.com \
  --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9 && \
  echo "deb https://mirrors.dotsrc.org/cran/bin/linux/ubuntu bionic-cran35/" > /etc/apt/sources.list.d/cran.list && \
  apt-get update && \
  apt-get -y install --no-install-recommends --no-install-suggests \
    libcurl4-openssl-dev \
    libssl-dev \
    libxml2-dev \
    libxt-dev \
    libcairo2-dev \
    r-base=${R_BASE_VERSION}* \
    r-base-core=${R_BASE_VERSION}* \
    r-cran-class \
    r-cran-nnet \
    r-base-dev=${R_BASE_VERSION}* \
    r-base-html=${R_BASE_VERSION}* \
    r-recommended=${R_BASE_VERSION}*
   R -e "install.packages(c('data.table', 'tidyverse', 'rmarkdown'), Ncpus = 7)"

   # Add a default CRAN mirror
  echo "options(repos = c(CRAN = 'https://cran.rstudio.com/'), download.file.method = 'libcurl')" >> /usr/lib/R/etc/Rprofile.site

  # Add a directory for host R libraries
  mkdir -p /library
  echo "R_LIBS_SITE=/library:\${R_LIBS_SITE}" >> /usr/lib/R/etc/Renviron.site

  # Clean up
  rm -rf /var/lib/apt/lists/*
