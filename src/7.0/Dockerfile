FROM chesszebra/php:7.0-cli

# Run as root
USER root:root

# Configure the system
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
 	curl \
 	git \
 	openssh-client \
	wget \
	zlib1g-dev

# Install the zip extension
RUN docker-php-ext-install zip \
 && docker-php-ext-enable zip

# Install Composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
 && php -r "if (hash_file('SHA384', 'composer-setup.php') === '669656bab3166a7aff8a7506b8cb2d1c292f042046c5a994c43155c0be6190fa0355160742ab2e1c88d40d5be660b410') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
 && php composer-setup.php \
 && php -r "unlink('composer-setup.php');" \
 && mv composer.phar /usr/local/bin/composer \
 && composer self-update \
 && chown php:php /usr/local/bin/composer

# Run the container as the configured user
USER php:php

# Execute composer
ENTRYPOINT ["composer"]
