FROM phusion/baseimage:0.9.4

# Set correct environment variables.
ENV HOME /root

# Regenerate SSH host keys. baseimage-docker does not contain any, so you
# have to do that yourself. You may also comment out this instruction; the
# init system will auto-generate one during boot.
RUN /etc/my_init.d/00_regen_ssh_host_keys.sh

# ...put your own build instructions here...
# Add java repository
RUN apt-get update && apt-get install -y python-software-properties && add-apt-repository -y ppa:webupd8team/java && apt-get update 
# && apt-get upgrade -y

# Install Oracle Java silently. Use the debconf autoaccept hack
RUN echo debconf shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && echo debconf shared/accepted-oracle-license-v1-1 seen true | debconf-set-selections && apt-get install -y oracle-java6-installer

# Do final package installs then clean up APT when done.
RUN apt-get install -y supervisor && mkdir -p /var/run/sshd && mkdir -p /var/run/supervisor

# Get and unzip pentaho BI server. Eventually, use this wget location: http://softlayer-ams.dl.sourceforge.net/project/pentaho/Business%20Intelligence%20Server/4.8.0-stable/biserver-ce-4.8.0-stable.tar.gz
RUN wget http://192.168.239.243:8011/biserver-ce-4.8.0-stable.tar.gz && mkdir /opt/pentaho && tar xvzf biserver-ce-4.8.0-stable.tar.gz -C /opt/pentaho && rm -f biserver-ce-4.8.0-stable.tar.gz && rm -f /opt/pentaho/biserver-ce/promptuser.sh

# Setup supervisord to run pentaho
ADD supervisor.conf /etc/supervisor.conf

# Enable SSH
# [delete mkdir]
ADD id_rsa.pub /tmp/id_rsa.pub
RUN cat /tmp/id_rsa.pub >> /root/.ssh/authorized_keys && rm -f /tmp/id_rsa.pub

# Clean up
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# sudo docker run -p 8080? -p 8099? -t -i mattdoka/pentaho {basic}
# OR sudo docker run mattdoka/pentaho
# Modify with: docker run YOUR_IMAGE /sbin/my_init -- COMMAND ARGUMENTS ...
#
# Use baseimage-docker's init system.
EXPOSE 8080 8099

CMD ["/sbin/my_init"]
