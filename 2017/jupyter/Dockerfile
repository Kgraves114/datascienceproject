FROM jupyter/datascience-notebook
MAINTAINER chapmanbe <brian.chapman@utah.edu>
USER root
# for physionet instructions see
# https://www.physionet.org/physiotools/wfdb-linux-quick-start.shtml
# dependencies for spell nbextensions (including spell check) and other requirements

RUN apt-get update && apt-get upgrade -y && apt-get install -y \
      tree \
      graphviz\
      less \
      openssh-client \
      gcc \
      libcurl4-openssl-dev \
      libexpat1-dev \
      libnetcdff-dev \
      locales-all \
      libxrender-dev \
      libfontconfig-dev \
      libffi-dev \
      rsync \
      mongodb-clients \
      libmariadb-client-lgpl-dev \
  && rm -rf /var/lib/apt/lists/*


# Install physiosnet software
WORKDIR /opt/wfdb
RUN wget https://www.physionet.org/physiotools/wfdb.tar.gz && \
    tar xvfz wfdb.tar.gz

WORKDIR /opt/wfdb/wfdb-10.6.0
RUN ./configure && \
    make install
WORKDIR  /opt
RUN rm -r /opt/wfdb

WORKDIR /home/jovyan

# Set default editor to vim
RUN update-alternatives --set editor /usr/bin/vim.basic

USER jovyan

RUN conda install -c damianavila82 rise -y

RUN git clone https://github.com/Calysto/notebook-extensions.git
WORKDIR /home/jovyan/notebook-extensions
RUN jupyter nbextension install calysto --user && \
    jupyter nbextension enable calysto/document-tools/main && \
    jupyter nbextension enable calysto/cell-tools/main && \
    jupyter nbextension enable calysto/spell-check/main && \
    jupyter nbextension enable calysto/publish/main && \
    jupyter nbextension enable calysto/submit/main && \
    jupyter nbextension enable calysto/annotate/main

RUN conda update conda -y && conda install -y \
    mpld3 \
    pip=9.0.1\
    pymysql \
    postgresql \
    gensim \
    biopython \
    pytables \
    scikit-image \
    pymongo \
    spacy

RUN conda install -y  -c  https://conda.anaconda.org/simpleitk SimpleITK
RUN conda install -y  -c conda-forge nbstripout


WORKDIR /home/jovyan
ADD decart_welcome.ipynb /home/jovyan/work/decart_welcome.ipynb
ADD media /home/jovyan/work/media
ADD dbmi_welcome.ipynb /home/jovyan/work/dbmi_welcome.ipynb
RUN conda install -y -c conda-forge nbgrader
# RUN nbstripout --install
CMD ["start-notebook.sh"]
