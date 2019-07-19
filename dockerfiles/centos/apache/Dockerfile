FROM safarov/base:latest
MAINTAINER Pulp Team <pulp-list@redhat.com>

EXPOSE 80 443

RUN yum update -y && yum install -y make && \
    yum clean all && \
    pulp-gen-key-pair && \
    pulp-gen-ca-certificate && \
    cp /etc/pki/pulp/ca.crt /etc/pki/tls/certs/localhost.crt && \
    cp /etc/pki/pulp/ca.key /etc/pki/tls/private/localhost.key

CMD [ "httpd", "-D", "FOREGROUND" ]
