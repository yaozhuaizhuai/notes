FROM jenkins/jnlp-slave
MAINTAINER zul@yingkelecai.com

ADD apache-maven-3.6.0-bin.tar.gz /usr/local/
ADD jdk-8u191-linux-x64.tar.gz /usr/local/

ENV MAVEN_HOME /usr/local/apache-maven-3.6.0
ENV JAVA_HOME  /usr/local/jdk1.8.0_191
ENV CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV PATH ${JAVA_HOME}/bin:${MAVEN_HOME}/bin:${PATH}

COPY libltdl.so.7 /usr/lib64/libltdl.so.7
COPY libltdl.so.7 /usr/lib/libltdl.so.7
USER root