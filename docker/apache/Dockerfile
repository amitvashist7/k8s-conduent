# Docker Sample Image
# Ubuntu with apache

# Image Defination 
FROM ubuntu:16.04

# Maintainer
MAINTAINER Amit Vashist <amitvashist7@outlook.com>

# RUN  Apt-get
RUN apt-get update

# Install Apache Packages
RUN apt-get install apache2 -y 

# Excute the Service
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
