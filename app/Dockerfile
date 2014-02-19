FROM phusion/baseimage:0.9.4
# USAGE: sudo docker run mattdoka/pentaho

# Set correct environment variables.
ENV HOME /root

# Regenerate SSH host keys. baseimage-docker does not contain any
RUN /etc/my_init.d/00_regen_ssh_host_keys.sh

# Add java repository
RUN apt-get update && apt-get install -y python-software-properties && add-apt-repository -y ppa:webupd8team/java && apt-get update 

# Install Oracle Java silently. Use the debconf autoaccept hack
RUN echo debconf shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && echo debconf shared/accepted-oracle-license-v1-1 seen true | debconf-set-selections && apt-get install -y oracle-java6-installer

# Do final package installs then clean up APT when done.
RUN apt-get install -y supervisor && mkdir -p /var/run/sshd && mkdir -p /var/run/supervisor

# Get and unzip pentaho BI server
RUN wget http://softlayer-ams.dl.sourceforge.net/project/pentaho/Business%20Intelligence%20Server/4.8.0-stable/biserver-ce-4.8.0-stable.tar.gz && mkdir /opt/pentaho && tar xvzf biserver-ce-4.8.0-stable.tar.gz -C /opt/pentaho && rm -f biserver-ce-4.8.0-stable.tar.gz && rm -f /opt/pentaho/biserver-ce/promptuser.sh

# Make sure Pentaho starts up when docker container is run
RUN mkdir -p /etc/my_init.d
ADD pentaho.sh /etc/my_init.d/pentaho.sh

# Enable SSH
ADD id_rsa.pub /tmp/id_rsa.pub
RUN cat /tmp/id_rsa.pub >> /root/.ssh/authorized_keys && rm -f /tmp/id_rsa.pub

# Clean up
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

EXPOSE 8080 8099

# Use baseimage-docker's init system.
CMD ["/sbin/my_init"]
