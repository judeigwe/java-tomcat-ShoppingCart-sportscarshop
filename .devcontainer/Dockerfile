FROM ubuntu:20.04

# Set non-interactive mode during build
ARG DEBIAN_FRONTEND=noninteractive

# Set Bash as the default shell
SHELL ["/bin/bash", "-c"]

# Install required packages
RUN apt-get update && apt-get install -y \
    curl \
    net-tools \
    wget \
    openjdk-17-jdk \
    unzip \
    && rm -rf /var/lib/apt/lists/*

# Set JAVA_HOME environment variable
ENV JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64

# Create a user called tomcat
RUN useradd -m -U -s /bin/bash tomcat

# Set the working directory
WORKDIR /home/tomcat

# Download and install Gradle
ENV GRADLE_VERSION=8.0.2
RUN wget https://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip && \
    unzip -d /opt/ gradle-${GRADLE_VERSION}-bin.zip && \
    rm gradle-${GRADLE_VERSION}-bin.zip

# Set Gradle environment variables
ENV GRADLE_HOME=/opt/gradle-${GRADLE_VERSION}
ENV PATH=$PATH:$GRADLE_HOME/bin

# Install Tomcat
ENV TOMCAT_VERSION=10.0.8
RUN wget https://archive.apache.org/dist/tomcat/tomcat-10/v${TOMCAT_VERSION}/bin/apache-tomcat-${TOMCAT_VERSION}.tar.gz && \
    tar xvzf apache-tomcat-${TOMCAT_VERSION}.tar.gz && \
    mv apache-tomcat-${TOMCAT_VERSION} /opt && \
    rm apache-tomcat-${TOMCAT_VERSION}.tar.gz && \
    chown -R tomcat:tomcat /opt/apache-tomcat-${TOMCAT_VERSION}

# Set Tomcat environment variables
ENV CATALINA_HOME=/opt/apache-tomcat-${TOMCAT_VERSION}
ENV PATH=$PATH:$CATALINA_HOME/bin

# Set permissions for Tomcat directories
RUN chown -R tomcat:tomcat $CATALINA_HOME/bin && \
    chown -R tomcat:tomcat $CATALINA_HOME/conf

# Switch to the tomcat user
USER tomcat

# Create the app directory
RUN mkdir app

# Set the work directory
WORKDIR /home/tomcat/app

# Copy files from the current workspace to the app directory
COPY ../ /home/tomcat/app

# WORKDIR /

# Expose port 8080
EXPOSE 8080

# # Start Tomcat
# CMD /opt/tomcat/bin/catalina.sh run