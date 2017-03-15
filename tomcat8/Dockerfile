FROM anapsix/alpine-java:8_jdk_unlimited

MAINTAINER lanhuai<lanhuai@gmail.com>

# alpine 的时区配置不起作用，主要依靠JDK的参数设置
# -Duser.language=zh -Duser.country=CN -Duser.timezone=Asia/Shanghai

# Expose web port
EXPOSE 8080

# Tomcat Version
ENV TOMCAT_VERSION_MAJOR 8
ENV TOMCAT_VERSION_FULL  8.5.12

# Download and install
# Add USTC repo
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/' /etc/apk/repositories \
#
# Set timezone
    && apk add -U tzdata \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && apk del tzdata \
#cp /usr/share/zoneinfo/Europe/Brussels /etc/localtime
#echo "Europe/Brussels" >  /etc/timezone
#
# Sepcify ntp server
#    && sed -i 's/pool.ntp.org/cn.pool.ntp.org/' /etc/init.d/ntpd \
  && apk add -U curl &&\
  curl -LO https://archive.apache.org/dist/tomcat/tomcat-${TOMCAT_VERSION_MAJOR}/v${TOMCAT_VERSION_FULL}/bin/apache-tomcat-${TOMCAT_VERSION_FULL}.tar.gz &&\
  curl -LO https://archive.apache.org/dist/tomcat/tomcat-${TOMCAT_VERSION_MAJOR}/v${TOMCAT_VERSION_FULL}/bin/apache-tomcat-${TOMCAT_VERSION_FULL}.tar.gz.md5 &&\
  md5sum -c apache-tomcat-${TOMCAT_VERSION_FULL}.tar.gz.md5 &&\
  gunzip -c apache-tomcat-${TOMCAT_VERSION_FULL}.tar.gz | tar -xf - -C /opt &&\
  rm -f apache-tomcat-${TOMCAT_VERSION_FULL}.tar.gz apache-tomcat-${TOMCAT_VERSION_FULL}.tar.gz.md5 &&\
  ln -s /opt/apache-tomcat-${TOMCAT_VERSION_FULL} /opt/tomcat &&\
  rm -rf /opt/tomcat/webapps/examples /opt/tomcat/webapps/docs /opt/tomcat/webapps/ROOT &&\
  apk del curl &&\
  rm -rf /var/cache/apk/*

# Configuration
#ADD tomcat-users.xml /opt/tomcat/conf/
#RUN sed -i 's/52428800/5242880000/g' /opt/tomcat/webapps/manager/WEB-INF/web.xml

# Set environment
ENV CATALINA_HOME /opt/tomcat
ENV PATH $PATH:$CATALINA_HOME/bin
ENV TZ "Asia/Shanghai"

WORKDIR $CATALINA_HOME