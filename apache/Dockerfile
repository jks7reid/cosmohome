FROM php:5.6.10-apache

MAINTAINER Marius Millea <mariusmillea@gmail.com>

#install packages 
RUN apt-get update && apt-get install -y \
        cron \
        git \
        mysql-client \
        python \
        python-matplotlib \
        python-mysqldb \
        python-numpy \
        python-scipy \
        python-yaml \
        rsyslog \
        supervisor \
        vim \
    && curl -L http://antiyawn.com/uploads/Humor-Sans-1.0.ttf > /usr/share/matplotlib/mpl-data/fonts/ttf/Humor-Sans.ttf

#configure server
RUN docker-php-ext-install mysqli 
RUN a2enmod cgi

#see https://github.com/docker/docker/issues/14651
RUN rm -rf /root && mkdir /root && chmod 755 /root

#so that www-data can read/write boinc server files (boinc server runs as root)
RUN adduser www-data root
COPY .bashrc /root/.bashrc
RUN echo "umask 0002" >> /etc/apache2/envvars

#for exec'ing in
WORKDIR /root/projects/cosmohome

RUN ln -s /root/projects/cosmohome/cosmohome.httpd.conf /etc/apache2/sites-enabled

#set up supervisor to run
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
CMD ["/usr/bin/supervisord"]
