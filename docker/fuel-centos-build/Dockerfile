#fuel/centos-docker-build
FROM centos:centos6

MAINTAINER Matthew Mosesohn mmosesohn@mirantis.com

RUN echo -e "[nailgun]\nname=Nailgun Local Repo\nbaseurl=http://$(route -n | awk '/^0.0.0.0/ { print $2 }'):_PORT_/os/x86_64/\ngpgcheck=0" > /etc/yum.repos.d/nailgun.repo;yum clean all;yum --quiet install -y sudo ami-creator livecd-tools python-imgcreate python-setuptools

ADD start.sh /usr/local/bin/start.sh
ADD fuel-centos.ks /root/fuel-centos.ks

RUN chmod 755 /usr/local/bin/start.sh

RUN sed -i '/requiretty/s/^/#/g' /etc/sudoers;touch /etc/sysconfig/network;mkdir -p /var/lib/hiera && touch /var/lib/hiera/common.yaml

#Replace repo and port in makefile
RUN sed -i "s/^url .*/url --url=http:\/\/$(route -n | awk '/^0.0.0.0/ { print $2 }'):_PORT_\/os\/x86_64\//" /root/fuel-centos.ks
RUN sed -i "s/^repo .*/repo --name='Nailgun' --baseurl=http:\/\/$(route -n | awk '/^0.0.0.0/ { print $2 }'):_PORT_\/os\/x86_64\//" /root/fuel-centos.ks

CMD ["/usr/local/bin/start.sh"]

