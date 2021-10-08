# Docker base image
# FROM achiral/bioc:latest
FROM bioconductor/bioconductor_docker

# information
LABEL version="2021.10"
LABEL description="Docker RStudio bioconductor"
LABEL maintainer="https://hub.docker.com/u/bioconductor"

# update/install apt package and Japanese font (IPAex)
RUN apt-get update \
  && apt-get install -y --no-install-recommends \
    ssh \
    libxt-dev \
    libxml2-dev \
    libgit2-dev \
    fonts-ipaexfont \
  && apt-get clean

# OS env JP locale (Character, DateTime)
ENV LANG ja_JP.UTF-8
ENV LC_ALL ja_JP.UTF-8
RUN sed -i '$d' /etc/locale.gen \
  && echo "ja_JP.UTF-8 UTF-8" >> /etc/locale.gen \
    && locale-gen ja_JP.UTF-8 \
    && /usr/sbin/update-locale LANG=ja_JP.UTF-8 LANGUAGE="ja_JP:ja"
RUN /bin/bash -c "source /etc/default/locale"
RUN ln -sf  /usr/share/zoneinfo/Asia/Tokyo /etc/localtime

# install Japanese font
RUN apt-get update && apt-get install -y \
  fonts-ipaexfont \
  fonts-noto-cjk

ENV R_REMOTES_NO_ERRORS_FROM_WARNINGS=true

# change directory
WORKDIR /home/rstudio/rproject

# Install R packages (remotes, renv, BiocManager, devtools)
# RUN install2.r pkgname1 pkgname2
# RUN R -e "install.packages('vector_of_package_names')"
# example code (below):
RUN install2.r --error --skipinstalled\
    BiocManager devtools githubinstall pacman remotes ranger
RUN Rscript -e "BiocManager::install(update = TRUE, ask=FALSE, Ncpus=2)"
RUN Rscript -e "BiocManager::install('DEP')"
# RUN Rscript -e "BiocManager::install('limma')"

# set global options
COPY --chown=rstudio:rstudio . /home/rstudio/