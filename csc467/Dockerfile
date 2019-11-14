FROM debian:jessie

MAINTAINER LINH B NGO <lngo@wcupa.edu>


RUN apt-get update

# ANACONDA3
RUN apt-get update && \
    apt-get install -y curl build-essential libpng12-dev libffi-dev  && \
    apt-get clean && \
    rm -rf /var/tmp /tmp /var/lib/apt/lists/*

#JAVA
RUN curl -sSL -o OpenJDK8U-jdk_x64_linux_hotspot_8u232b09.tar.gz https://github.com/AdoptOpenJDK/openjdk8-binaries/releases/download/jdk8u232-b09/OpenJDK8U-jdk_x64_linux_hotspot_8u232b09.tar.gz
RUN tar xzf OpenJDK8U-jdk_x64_linux_hotspot_8u232b09.tar.gz -C /opt
ENV JAVA_HOME="/opt/jdk8u232-b09"
ENV PATH="$JAVA_HOME/bin:$PATH"

ENV CONDA_DIR="/conda" 
ENV PATH="$CONDA_DIR/bin:$PATH"
RUN curl -sSL -o installer.sh https://repo.continuum.io/archive/Anaconda3-2019.10-Linux-x86_64.sh && \
    bash /installer.sh -b -f && \
    rm /installer.sh

ENV PATH "$PATH:/root/anaconda3/bin"

# SPARK
ARG SPARK_ARCHIVE=http://mirrors.ocf.berkeley.edu/apache/spark/spark-2.4.4/spark-2.4.4-bin-hadoop2.7.tgz
RUN curl -s $SPARK_ARCHIVE | tar -xz -C /usr/local/

ENV SPARK_HOME /usr/local/spark-2.4.4-bin-hadoop2.7
ENV PATH $PATH:$SPARK_HOME/bin

# CUSTOM CONDA-ENV
RUN conda create -n pyspark python=3.7
RUN ["/bin/bash", "-c", "source activate pyspark; apt-get install libstdc++; conda install jupyter;conda install ipykernel; python -m ipykernel install --user --name pyspark --display-name pyspark; source deactivate"]

# JUPYTER CONFIG
COPY jupyter_notebook_config.py /root/.jupyter/
EXPOSE 8888

# CODE DIR 
COPY notebooks /notebooks

# ENVIRONMENT VARIABLES
ENV JAVA_HOME /opt/jdk8u232-b09
ENV PYSPARK_PYTHON /root/anaconda3/envs/pyspark/bin/python 
# ENV PYSPARK_DRIVER_PYTHON_OPTS notebook
# ENV PYSPARK_DRIVER_PYTHON jupyter
ENV PYTHONPATH /usr/local/spark-2.4.4-bin-hadoop2.7/python/lib/py4j-0.10.7-src.zip:/usr/local/spark-2.4.4-bin-hadoop2.7/python:PYSPARK_DRIVER_PYTHON=ipython

WORKDIR "/notebooks"
CMD ["/bin/bash", "-c", "jupyter notebook --allow-root"]
