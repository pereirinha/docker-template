FROM johnpbloch/phpfpm:7.0

RUN apt-get update \
	&& apt-get install -y libgearman-dev unzip wget zip \
	&& apt-get clean \
	&& wget https://github.com/wcgallego/pecl-gearman/archive/master.zip \
	&& unzip master.zip \
	&& rm master.zip \
	&& ( \
		cd pecl-gearman-master \
		&& phpize \
		&& ./configure \
		&& make -j$(nproc) \
		&& make install \
	) \
	&& rm -r pecl-gearman-master \
	&& docker-php-ext-enable gearman

RUN curl -L https://phar.phpunit.de/phpunit.phar > /tmp/phpunit.phar \
	&& chmod +x /tmp/phpunit.phar \
	&& mv /tmp/phpunit.phar /usr/local/bin/phpunit

RUN apt-get install -y git subversion wget

RUN apt-get install -y nmap

RUN curl --location --output /usr/local/bin/mhsendmail https://github.com/mailhog/mhsendmail/releases/download/v0.2.0/mhsendmail_linux_amd64 \
	&& chmod +x /usr/local/bin/mhsendmail

RUN echo 'sendmail_path="/usr/local/bin/mhsendmail --smtp-addr=mailhog:1025 --from=no-reply@docker.dev"' > /usr/local/etc/php/conf.d/mailhog.ini

CMD ["php-fpm"]

EXPOSE 9000 1025