FROM centos:7
MAINTAINER Fabrice Jammes <fabrice.jammes@in2p3.fr>

RUN yum -y update
RUN yum -y install curl python make
RUN yum -y install flex 
RUN yum -y install bison 
RUN yum -y install gcc-c++ 
RUN yum -y install perl 

RUN groupadd qserv && \
    useradd -m -g qserv qserv && \
    mkdir /qserv && \
    chown qserv:qserv /qserv

USER qserv

WORKDIR /qserv

# Copying only install-stack.sh to avoid re-installing if
# other files changes in scripts/
COPY scripts/install-stack.sh scripts/install-stack.sh

# Hack to enable diagnose of stack install failure
RUN /bin/bash /qserv/scripts/install-stack.sh 2>&1 /qserv/STACK_INSTALL.out || echo 'INSTALL FAILED' > /qserv/STACK_INSTALL.err && cat $(find /qserv/stack -name *.log)

COPY scripts/*.sh scripts/

USER root

RUN chown -R qserv:qserv /qserv/scripts/
