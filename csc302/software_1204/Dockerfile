FROM linhbngo/ubuntu:12.04

RUN apt-get update
RUN apt-get install -y wget sudo nano
RUN wget --no-check-certificate https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh \
    && bash Miniconda3-latest-Linux-x86_64.sh -b -p /opt/miniconda \
    && echo "PATH=/opt/miniconda/bin:$PATH" >> /etc/environment \
    && . /etc/environment \
    && pip install jupyter
RUN useradd -m -s /bin/bash seed \
    && echo "seed:dees" | chpasswd \
    && usermod -a -G sudo seed

CMD ["su", "-", "seed", "-c", "/opt/miniconda/bin/jupyter notebook --NotebookApp.token='' --ip=0.0.0.0 --no-browser"]
