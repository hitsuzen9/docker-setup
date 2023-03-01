FROM debian:buster

LABEL maintainer="XuanSon <xuansonitvn@gmail.com>" \
	org.label-schema.name="Debian with PHP 8.1, optimised using PHP-FPM"
LABEL version="1.0"

# Set up locales
ENV LANG C.UTF-8
ENV LANGUAGE C.UTF-8
ENV LC_ALL C.UTF-8

# Initial apt update
RUN apt-get update && apt-get -y install wget curl git gnupg \
	software-properties-common
# python-software-properties

# Install common / shared packages
RUN apt-get -y install lsb-release apt-transport-https ca-certificates
RUN	wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
RUN	echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | tee /etc/apt/sources.list.d/php.list

RUN apt-get update && apt-get install -y \
    zip \
    unzip \
    vim \
    locales \
    dialog \
    gcc \
    musl-dev \
    libffi-dev \
    iputils-ping \
    net-tools \
    telnet \
	python \
    python-dev \
	python-pip

# Install PHP 8.1
RUN	apt-get -y install \
	php8.1 php8.1-cgi php8.1-cli php8.1-common php8.1-curl php8.1-dev php8.1-gd php8.1-gmp php8.1-ldap php8.1-mysql \
	php8.1-odbc php8.1-opcache php8.1-pgsql php8.1-pspell php8.1-readline php8.1-sqlite3 php8.1-tidy \
	php8.1-xml php8.1-xmlrpc php8.1-xdebug \
	libphp8.1-embed php8.1-bcmath php8.1-bz2 php8.1-enchant php8.1-fpm php8.1-imap php8.1-interbase php8.1-intl php8.1-mbstring php8.1-redis \
	php8.1-phpdbg php8.1-soap php8.1-sybase php8.1-xsl php8.1-zip php8.1-dba \
	# php8.1-mysqlnd-ms  php8.1-recode \
    php8.1-memcached php8.1-mongodb php8.1-imagick

# Install Composer globally
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ && mv /usr/bin/composer.phar /usr/bin/composer

# Set time asia/ho_chi_minh
RUN \
    apt-get -y install tzdata && \
    ln -sf /usr/share/zoneinfo/Asia/Ho_Chi_Minh /etc/localtime && date

# Install Supervisor
RUN apt-get -y install supervisor
RUN mkdir -p /var/log/cache

RUN	apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
RUN apt-get remove --purge --auto-remove -y && rm -rf /var/lib/apt/lists/* /etc/apt/sources.list.d/*.list

# if we have leftovers from building, let's purge them (including extra, unnecessary build deps)
RUN    apt-get purge -y --auto-remove

RUN update-alternatives --set php /usr/bin/php8.1

ADD ./conf.d/www.conf /etc/php/8.1/fpm/pool.d/www.conf
ADD ./conf.d/php.ini /etc/php/8.1/fpm/php.ini
ADD ./conf.d/php-fpm.conf /etc/php/8.1/fpm/php-fpm.conf
ADD ./supervisor/supervisord.conf /etc/supervisor/supervisord.conf

WORKDIR /var/www/html

EXPOSE 9075

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]