FROM		nginx		
COPY            wiki.conf      /etc/nginx/conf.d/default.conf
RUN		apt update -y && apt install -y wget mariadb-client nano php-mbstring gzip unzip php-fpm php-xml php-intl php-mysql
RUN		usermod -a -G www-data nginx
ADD		https://releases.wikimedia.org/mediawiki/1.36/mediawiki-1.36.0-rc.0.tar.gz /
RUN		tar xzvf /mediawiki-1.36.0-rc.0.tar.gz && mv /mediawiki-1.36.0-rc.0 /usr/share/nginx/wiki && chown -R www-data.www-data /usr/share/nginx/wiki
VOLUME		/var/log/nginx /usr/share/nginx/wiki /etc/nginx/conf.d/
CMD		service	php8.2-fpm start && nginx -g "daemon off;"
