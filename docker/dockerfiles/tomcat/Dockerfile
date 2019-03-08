FROM centos:7
MAINTAINER zul@yingkelecai.com

ADD jdk-8u191-linux-x64.tar.gz /usr/local/
ADD apache-tomcat-8.5.35.tar.gz /usr/local/

ENV JAVA_HOME  /usr/local/jdk1.8.0_191
ENV CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV CATALINA_HOME /usr/local/apache-tomcat-8.5.35
ENV CATALINA_BASE /usr/local/apache-tomcat-8.5.35
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin
ENV LANG C.UTF-8

RUN echo "Asia/shanghai" > /etc/timezone

WORKDIR $CATALINA_HOME

USER root

EXPOSE 8080
CMD ["catalina.sh", "run"]