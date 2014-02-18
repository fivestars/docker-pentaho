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

# Install Oracle Java silently. Use the debconf autoaccept hack
RUN echo debconf shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && echo debconf shared/accepted-oracle-license-v1-1 seen true | debconf-set-selections && apt-get install -y oracle-java6-installer

# Do final package installs then clean up APT when done.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Get and unzip pentaho BI server
RUN wget http://localhost:8011/biserver-ce-4.8.0-stable.tar.gz && mkdir /etc/pentaho && tar xvzf biserver-ce-4.8.0-stable.tar.gz -C /etc/pentaho && rm biserver-ce-4.8.0-stable.tar.gz
