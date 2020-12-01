FROM ubuntu:18.04
LABEL author=gijs@pythonic.nl maintainer=andres.kesk@gmail.com
ENV DEBIAN_FRONTEND noninteractive

## Clear python2
RUN apt-get autoremove python* -y

## install requirements
RUN apt-get update
RUN apt-get install -y curl erlang git make python3 python3-pip openssh-server
RUN ln -s /usr/bin/python3 /usr/bin/python
RUN ln -s /usr/bin/pip3 /usr/bin/pip
RUN pip install supervisor

## add user for disco
RUN adduser --system disco --shell /bin/bash

## setup ssh
RUN echo root:disco | chpasswd
RUN mkdir -p /var/run/sshd

## passwordless login for docker 
RUN mkdir -p /home/disco/.ssh
RUN ssh-keygen -N '' -f /home/disco/.ssh/id_dsa
RUN cat /home/disco/.ssh/id_dsa.pub >> /home/disco/.ssh/authorized_keys
RUN echo -n "localhost " > /home/disco/.ssh/known_hosts
RUN cat /etc/ssh/ssh_host_rsa_key.pub >> /home/disco/.ssh/known_hosts
RUN chown disco -R /home/disco/.ssh

## install disco
ENV DISCO_HOME=/disco
# DISCO_HOME also set in supervisor.conf
RUN git clone https://github.com/andres-k/disco.git ${DISCO_HOME}
WORKDIR ${DISCO_HOME}
RUN make install
RUN cd ${DISCO_HOME}/lib && python setup.py install
RUN chown -R disco /usr/var/disco

## Setup folders for logging and dfds
RUN mkdir -p /var/log/disco
RUN mkdir -p /dfds
RUN touch /var/log/disco.log

## configure the supervisor which will start ssh & docker
ADD supervisor.conf /etc/supervisor/supervisord.conf
RUN sed -i 's/without-password/yes/g' /etc/ssh/sshd_config

EXPOSE 22
EXPOSE 8990
EXPOSE 8989
CMD ["/usr/local/bin/supervisord"]
