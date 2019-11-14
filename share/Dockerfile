ARG SHARE_TAG
FROM alfresco/alfresco-share:${SHARE_TAG}

ARG TOMCAT_DIR=/usr/local/tomcat

USER root

# Install deltarpm
RUN yum install -y deltarpm

# Install modules and addons
RUN mkdir -p $TOMCAT_DIR/amps
COPY modules/amps $TOMCAT_DIR/amps
COPY modules/jars $TOMCAT_DIR/webapps/share/WEB-INF/lib

# Remove CSRF policy and modify APIs to comunicate to ACS with https
COPY share-config-custom.xml $TOMCAT_DIR/shared/classes/alfresco/web-extension/share-config-custom.xml

RUN java -jar $TOMCAT_DIR/alfresco-mmt/alfresco-mmt*.jar install \
    $TOMCAT_DIR/amps $TOMCAT_DIR/webapps/share -directory -nobackup -force

# Install czech language pack
RUN yum -y update && \
 yum -y install wget && \
 yum clean all && \
 set -x \
    && wget http://alf-trans.ossportal.org/files/share-cs.jar -P $TOMCAT_DIR/lib

