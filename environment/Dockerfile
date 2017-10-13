FROM debian:8.5

MAINTAINER Soeren Stelzer (soeren.stelzer@gmx.de)

ARG CONDA_VERSION=5.0.0.1
ARG CONDA_FILE=Anaconda3-${CONDA_VERSION}-Linux-x86_64.sh
ARG CONDA_URL=https://repo.continuum.io/archive/${CONDA_FILE}

ARG TENSORFLOW_URL=https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.3.0-cp36-cp36m-linux_x86_64.whl

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ENV TF_LOG_PATH=/opt/tf-logs

RUN apt-get update --fix-missing && apt-get install -y wget bzip2 ca-certificates \
    libglib2.0-0 libxext6 libsm6 libxrender1 \
    git mercurial subversion

RUN echo 'export PATH=/opt/conda/bin:$PATH' > /etc/profile.d/conda.sh && \
    wget --quiet ${CONDA_URL} -O ~/anaconda.sh && \
    /bin/bash ~/anaconda.sh -b -p /opt/conda && \
    rm ~/anaconda.sh

ENV PATH /opt/conda/bin:$PATH
    
RUN pip install --ignore-installed --upgrade ${TENSORFLOW_URL} && \
    export TF_CPP_MIN_LOG_LEVEL=2

RUN conda install jupyter && \
    mkdir -p /opt/notebooks ${TF_LOG_PATH}

VOLUME ["/opt/notebooks"]
EXPOSE 8888

ENTRYPOINT ["/opt/conda/bin/jupyter"]
CMD ["notebook", "--notebook-dir=/opt/notebooks", "--ip='*'","--port=8888", "--no-browser", "--allow-root" ]