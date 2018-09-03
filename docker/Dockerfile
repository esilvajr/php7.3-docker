FROM ubuntu:18.04
 
RUN apt-get update \ 
    && apt-get -y --no-install-recommends install ca-certificates vim wget gcc build-essential libxml2-dev libssl-dev libcurl4-openssl-dev pkg-config curl make libpq-dev libpspell-dev librecode-dev libcurl4-openssl-dev libxft-dev

#installing php7.3 from source
RUN wget https://downloads.php.net/~cmb/php-7.3.0beta2.tar.gz --no-check-certificate \
	&& tar zxvf php-7.3* && cd php-7.3.0beta3 \
	&& ./configure --enable-fpm --with-fpm-user=www-data --with-fpm-group=www-data --prefix=/etc/php/7.3 --enable-debug --with-mysqli --with-pgsql --with-pdo-mysql --with-pdo-pgsql --with-curl --enable-mbstring --with-openssl\
	&& make && make install \
	&& rm -rf /application/php-7.3*

WORKDIR "/application"

#configuring php-fpm
COPY php-fpm/overrides.conf /etc/php/7.3/etc/php-fpm.d/z-overrides.conf
COPY php-fpm/startup /usr/bin/php-fpm

RUN cp /etc/php/7.3/etc/php-fpm.conf.default /etc/php/7.3/etc/php-fpm.conf && ln -s /etc/php/7.3/bin/php /usr/bin/php

CMD /usr/bin/php-fpm

EXPOSE 9000

#installing composer
RUN curl -sS https://getcomposer.org/installer -o composer-setup.php && HASH=544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061 \
	&& php -r "if (hash_file('SHA384', 'composer-setup.php') === '$HASH') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
	&& php composer-setup.php --install-dir=/usr/local/bin --filename=composer\
	&& rm -rf /application/composer-setup.php