# This image provides a base for building and running microservice Java applications.
# It builds using maven and runs the resulting artifacts with "java -jar ..."

FROM s2i-centos7-java:latest

MAINTAINER Chen Feixiang <chenfeixiang@nationsky.com>

EXPOSE 8070

ENV MAVEN_VERSION=3.3.9

LABEL io.k8s.description="Platform for building java microservice applications on MS 1.0.Final" \
      io.k8s.display-name="MS 1.0.Final" \
      io.openshift.expose-services="8070:http" \
      io.openshift.tags="builder,ms,java" \
      io.openshift.s2i.destination="/opt/s2i/destination"

# Install Maven
# Speed up maven downloading by using: curl -v http://10.1.15.188:8000/apache-maven-$MAVEN_VERSION-bin.tar.gz
RUN (curl -v https://www.apache.org/dist/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz | \
    tar -zx -C /usr/local) && \
    ln -sf /usr/local/apache-maven-$MAVEN_VERSION/bin/mvn /usr/local/bin/mvn && \
    mkdir -p $HOME/.m2 && \
    mkdir -p /opt/s2i/destination

# Copy maven repo to speed up build time
COPY ./.m2/ $HOME/.m2
	
# Copy the S2I scripts from the specific language image to $STI_SCRIPTS_PATH
COPY ./s2i/bin/ $STI_SCRIPTS_PATH

RUN chown -R 1001:0 $HOME && \
    chmod -R g+rw /opt/s2i/destination
	chmod -R g+rxw $STI_SCRIPTS_PATH

USER 1001

CMD $STI_SCRIPTS_PATH/usage