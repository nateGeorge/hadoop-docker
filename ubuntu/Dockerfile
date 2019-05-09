# Ubuntu 18 LTS
FROM ubuntu:18.04

# Do not exclude man pages & other documentation
RUN rm /etc/dpkg/dpkg.cfg.d/excludes
# Reinstall all currently installed packages in order to get the man pages back
RUN apt-get update && \
    dpkg -l | grep ^ii | cut -d' ' -f3 | xargs apt-get install -y --reinstall && \
    rm -r /var/lib/apt/lists/*

# Install any needed packages specified in requirements.txt
RUN apt-get update
# install necessary packages
RUN apt-get install wget ssh pdsh openssh-server git python sudo man -y
# start SSH server
RUN /etc/init.d/ssh start
# Install java 8 -- newer versions don't necessarily work with Hadoop
RUN apt-get install openjdk-8-jdk -y

# Install Hadoop core
RUN wget http://apache.claz.org/hadoop/common/hadoop-3.1.2/hadoop-3.1.2.tar.gz
RUN tar -zxvf hadoop-3.1.2.tar.gz -C /opt
# set JAVA_HOME environment variable; necessary for Hadoop to function
RUN echo "export JAVA_HOME=/usr" >> /opt/hadoop-3.1.2/etc/hadoop/hadoop-env.sh
RUN echo "export JAVA_HOME=/usr" >> ~/.bashrc
# add hadoop to our path so we can run it easily
RUN echo "export PATH=/opt/hadoop-3.1.2/bin:$PATH" >> ~/.bashrc

# This would add hadoop user and add home directory for this user
# RUN groupadd -r hadoop && useradd -r -g hadoop hadoop
# RUN mkdir /home/hadoop
# RUN touch /home/hadoop/.bashrc

WORKDIR /home/root
# This creates a hadoop user and starts the container in the home directory
#USER hadoop
#WORKDIR /home/hadoop
