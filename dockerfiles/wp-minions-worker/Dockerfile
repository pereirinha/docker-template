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

RUN apt-get update \
	&& apt-get install -y supervisor \
	&& apt-get clean

RUN mkdir -p /var/log/supervisor
RUN mkdir -p /etc/supervisord.d

RUN curl --location --output /usr/local/bin/mhsendmail https://github.com/mailhog/mhsendmail/releases/download/v0.2.0/mhsendmail_linux_amd64 \
	&& chmod +x /usr/local/bin/mhsendmail

RUN echo 'sendmail_path="/usr/local/bin/mhsendmail --smtp-addr=mailhog:1025 --from=no-reply@docker.dev"' > /usr/local/etc/php/conf.d/mailhog.ini

CMD ["/usr/bin/supervisord", "-n"]
