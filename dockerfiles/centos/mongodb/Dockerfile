FROM fedora:28
MAINTAINER Pulp Team <pulp-list@redhat.com>

#RUN yum install -y epel-release && yum clean all

RUN  yum update -y && yum install -y mongodb-server && \
     yum clean all && \
     sed -e 's/fork: true/fork: false/'\
         -e 's/bindIp: 127.0.0.1,::1/bindIp: 0.0.0.0/' \
         -i /etc/mongod.conf

RUN  mkdir -p /var/lib/mongodb && \
     touch /var/lib/mongodb/.keep && \
     chown -R mongodb:mongodb /var/lib/mongodb

VOLUME ["/var/lib/mongodb"]

USER mongodb

#ADD mongodb.conf /etc/mongodb.conf

EXPOSE 27017
CMD ["/usr/bin/mongod", "--quiet", "--config", "/etc/mongod.conf", "run"]
