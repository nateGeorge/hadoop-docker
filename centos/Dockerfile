# CentOS 7.6
FROM centos:7.6.1810

# Set the working directory to /app
#WORKDIR /app

# Copy the current directory contents into the container at /app
#COPY . /app

# Install any needed packages specified in requirements.txt
RUN yum install epel-release -y
RUN yum update -y
#RUN reboot -r now
RUN yum install -y java-1.8.0-openjdk
RUN yum install wget
#RUN wget http://apache.claz.org/hadoop/common/hadoop-3.1.2/hadoop-3.1.2.tar.gz
#RUN sudo tar -zxvf hadoop-3.1.2.tar.gz -C /opt
#RUN sed -i 's/export JAVA_HOME=$/export JAVA_HOME=$(readlink -f /usr/bin/java | sed "s:bin/java::")/g' /opt/hadoop-2.7.3/etc/hadoop/hadoop-env.sh

# Make port 80 available to the world outside this container
#EXPOSE 80

# Define environment variable
#ENV NAME World

# Run app.py when the container launches
#CMD ["python", "app.py"]
