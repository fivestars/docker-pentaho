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

RUN apt-get update && apt-get install -y python-software-properties && add-apt-repository -y ppa:webupd8team/java && apt-get update

# RUN apt-get install oracle-java6-installer

# Clean up APT when done.
# RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
