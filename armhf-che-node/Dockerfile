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

# Install Node & co
RUN curl -sL https://deb.nodesource.com/setup_4.x | sudo bash - && \
    sudo apt-get install -y build-essential nodejs

EXPOSE 3000 5000 9000
RUN sudo npm install -g npm@latest
RUN sudo npm install --unsafe-perm -g gulp bower grunt grunt-cli yeoman-generator yo generator-angular generator-karma generator-webapp


# Execute further instructions as a user and login to terminal as user. Skip this instruction if there's already a user with UID 1000 in the image
USER user

# Expose 22 port for sshd
EXPOSE 22

# Start sshd and execute a non-terminating command
CMD sudo /usr/sbin/sshd -D && \
    tailf /dev/null





