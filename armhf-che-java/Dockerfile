# Inherit from an official image
FROM armv7/armhf-java8

# Executes a script to install packages including Java, openssh, sudo, as well as create a user with UID 1000

RUN sudo apt-get update
RUN sudo apt-get -y install --no-install-recommends \
        wget ca-certificates openssh-server sudo procps unzip git curl
RUN sudo mkdir /var/run/sshd && \
        sudo sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd && \
        sudo echo "%sudo ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

   
RUN sudo useradd -u 1000 -G users,sudo -d /home/user --shell /bin/bash  -m user && \
    sudo echo -e "secret\nsecret" | passwd user

# Install Maven
ENV MAVEN_VERSION=3.3.9
ENV M2_HOME=/home/user/apache-maven-$MAVEN_VERSION
ENV PATH=$M2_HOME/bin:$PATH


RUN mkdir /home/user/apache-maven-$MAVEN_VERSION && \
  wget -qO- "http://apache.ip-connect.vn.ua/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz" | tar -zx --strip-components=1 -C /home/user/apache-maven-$MAVEN_VERSION/


# Execute further instructions as a user and login to terminal as user. Skip this instruction if there's already a user with UID 1000 in the image
USER user

# Expose 22 port for sshd
EXPOSE 22

# Start sshd and execute a non-terminating command
CMD sudo /usr/sbin/sshd -D && \
    tailf /dev/null





