FROM centos:centos7
# args
ARG WEB_ROOT_DIRECTORY
ARG WEB_DOMAIN
ARG WEB_HOST_PORTNUM
ARG WEB_CONTAINER_PORTNUM
ARG WEB_HOST_PORTSSL
ARG WEB_CONTAINER_PORTSSL
# timezone
RUN cp -p -f /usr/share/zoneinfo/Japan /etc/localtime
# yum install
RUN yum -y update && yum -y install \
    epel-release \
    sudo \
    less \
    # network ss (instaed of netstat)
    iproute \
    # apache
    httpd-devel \
    # zip
    zip \
    unzip \
    # SSL
    openssl \
    mod_ssl \
    # make
    wget \
    make \
    gcc
# add last
RUN echo ServerName www.example.com:${WEB_CONTAINER_PORTNUM} >> /etc/httpd/conf/httpd.conf
# SSL
RUN mkdir -p /etc/ssl/private
# volume directory
RUN mkdir /template
RUN mkdir /var/www/${WEB_ROOT_DIRECTORY}
RUN mkdir /var/www/${WEB_ROOT_DIRECTORY}/web
RUN mkdir /var/www/${WEB_ROOT_DIRECTORY}/web/cgi-bin
RUN mkdir /workspace
# Perl
RUN mkdir /var/perl58
# Perl
WORKDIR /var/perl58/
RUN wget http://www.cpan.org/src/perl-5.8.9.tar.gz
RUN tar zxvf perl-5.8.9.tar.gz
WORKDIR /var/perl58/perl-5.8.9
## configure
RUN ./Configure -des -Accflags='-fPIC' -Dprefix=/usr/local/perl-5.8.9 -Dscriptdir=/usr/local/perl-5.8.9/bin -DDEBUGGING=-g -Duseshrplib -Dman1dir=none -Dman3dir=none -Duseithreads
## make
RUN make -ik test
RUN make install
## symboliclink
RUN ln -s /usr/local/perl-5.8.9/bin/perl /usr/local/bin/perl

# mod_perl
WORKDIR /var/perl58/
RUN wget https://dlcdn.apache.org/perl/mod_perl-2.0.12.tar.gz
RUN tar zxvf mod_perl-2.0.12.tar.gz
WORKDIR /var/perl58/mod_perl-2.0.12
## Makefile.PL
RUN perl Makefile.PL MP_APXS=/usr/bin/apxs MP_AP_CONFIGURE="prefix=/usr/local/mod_perl-2.0.12 -enable-so -enable-rewrite"
## make
RUN make
RUN make install