FROM centos:8

RUN dnf group install -y "Development Tools"
RUN dnf install -y man-pages
RUN yum update -y
RUN yum install -y wget openmpi-devel python3 \
    && ln -s /usr/bin/python /usr/bin/python3
RUN wget https://bitbucket.org/berkeleylab/upcxx/downloads/upcxx-2020.3.0.tar.gz
RUN tar xzf upcxx-2020.3.0.tar.gz
RUN cd upcxx-2020.3.0 \ 
    && ./configure --prefix=/opt/upcxx/2020.3.0/
RUN make all
RUN make install
RUN make tests_clean test_install
