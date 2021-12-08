FROM ubuntu:20.04
ENV container docker
RUN apt-get update -y && apt-get dist-upgrade -y

# Install system dependencies, you may not need all of these
RUN apt-get install -y --no-install-recommends ssh sudo libffi-dev systemd openssh-client

# Needed to run systemd
# VOLUME [ "/sys/fs/cgroup" ]
# Doesn't appear to be necessary? See comments

RUN apt-get -y install puppet

# Add vagrant user and key for SSH
RUN useradd --create-home -s /bin/bash vagrant
RUN echo -n 'vagrant:vagrant' | chpasswd
RUN echo 'vagrant ALL = NOPASSWD: ALL' > /etc/sudoers.d/vagrant
RUN chmod 440 /etc/sudoers.d/vagrant
RUN mkdir -p /home/vagrant/.ssh
RUN chmod 700 /home/vagrant/.ssh
RUN touch /home/vagrant/.ssh/authorized_keys
RUN echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDDbtyBn1R6Zrvs/Jry1azoDv7xNQ6TPvkU1dH1wt8O4ffshFMYWwPiT1zc/XH52S7HyGYO/CWNrZRkcyXjIYcqe0izw+AJLTm0s/EWv2R6qreBs93Q4RnABWxsmuEHJEA2bUnnGO69dfi5I60XSOgEGU5+lNI31jCFKa0h5jhRMt1EU9jXskC6o5N7cb6N8eXTAA7GqtvGAK1sAuSqK7iC2AI65wtPhONpAY3gnbewKDsc0N6/F37od2FlWaVIZySCPzpR0BQWuisANfaDqCIBVeentRU5rF2qnEETiww3UPGWzdyXXruIH/D3GEM8e4xWuJYoPN+GcdmMgv6CPm7APp3Kgx38M37Oc6rkmv6P6R9Kxp/EUeuUSlmXjz2RU7Gv9sg2KBWBZp1pLOJxWjVOe0DgxCmz6exZn9O3oT88i1YxWVHy1xPCW0YZOZKVs0W9Y3PGzI3C7Cf+9qfEvZGUktT1X0Ca0WZsR4bctkeSI8aMpjFQxObNyHqj7m/O6SL7r8RcZuf+G8pnwRNBXeiARv0Vz+Eys60edtH19OmCX8Ajnq9/+GVG91iwTogd2fGduhnuKVZABBn/kU0jTf+QfLtOaA0Gb0BdbkLPNo/bgBoNtV4IC7qobpBHLtkotV+3fCwrxLuTtF3wvB9si7fiqWt+cR7Mva0KCsVRSrXSkw==" > /home/vagrant/.ssh/authorized_keys
RUN echo "ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEA6NF8iallvQVp22WDkTkyrtvp9eWW6A8YVr+kz4TjGYe7gHzIw+niNltGEFHzD8+v1I2YJ6oXevct1YeS0o9HZyN1Q9qgCgzUFtdOKLv6IedplqoPkcmF0aYet2PkEDo3MlTBckFXPITAMzF8dJSIFo9D8HfdOV0IAdx4O7PtixWKn5y2hMNG0zQPyUecp4pzC6kivAIhyfHilFR61RGL+GPXQ2MWZWFYbAGjyiYJnAmCP3NOTd0jMZEnDkbUvxhMmBYSdETk1rRgm+R4LOzFUGaHqHDLKLX+FIPKcF96hrucXzcWyLbIbEgE98OHlnVYCzRdK8jlqm8tehUc9c9WhQ== wille.faler@gmail.com" >> /home/vagrant/.ssh/authorized_keys
RUN chmod 600 /home/vagrant/.ssh/authorized_keys
RUN chown -R vagrant:vagrant /home/vagrant/.ssh
RUN sed -i -e 's/Defaults.*requiretty/#&/' /etc/sudoers
RUN sed -i -e 's/\(UsePAM \)yes/\1 no/' /etc/ssh/sshd_config

# Start SSH
RUN mkdir /var/run/sshd
EXPOSE 22
RUN /usr/sbin/sshd

# Start Systemd (systemctl)
CMD ["/lib/systemd/systemd"]