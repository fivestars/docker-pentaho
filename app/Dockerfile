FROM phusion/baseimage:0.9.4

# Set correct environment variables.
ENV HOME /root

# Regenerate SSH host keys. baseimage-docker does not contain any, so you
# have to do that yourself. You may also comment out this instruction; the
# init system will auto-generate one during boot.
RUN /etc/my_init.d/00_regen_ssh_host_keys.sh

# Use baseimage-docker's init system.
CMD ["/sbin/my_init"]

# ...put your own build instructions here...
# Add java repository
RUN apt-get update && apt-get install -y python-software-properties && add-apt-repository -y ppa:webupd8team/java && apt-get update 
#&& apt-get upgrade -y

# Install Oracle Java silently. Use the debconf autoaccept hack
RUN echo debconf shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && echo debconf shared/accepted-oracle-license-v1-1 seen true | debconf-set-selections && apt-get install -y oracle-java6-installer

# Do final package installs then clean up APT when done.
RUN apt-get install -y supervisor && mkdir -p /var/run/sshd && mkdir -p /var/run/supervisor
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Get and unzip pentaho BI server. Eventually, use this wget location: http://softlayer-ams.dl.sourceforge.net/project/pentaho/Business%20Intelligence%20Server/4.8.0-stable/biserver-ce-4.8.0-stable.tar.gz
RUN wget http://192.168.239.243:8011/biserver-ce-4.8.0-stable.tar.gz && mkdir /etc/pentaho && tar xvzf biserver-ce-4.8.0-stable.tar.gz -C /etc/pentaho && rm biserver-ce-4.8.0-stable.tar.gz && rm /etc/pentaho/biserver-ce/promptuser.sh

ADD supervisord.conf /etc/supervisor/conf.d/supervisord.conf

RUN mkdir /etc/service/supervisorctl
ADD supervisorctl.sh /etc/service/supervisorctl/run

EXPOSE 8080, 8099
