FROM chesszebra/php:7.2-cli

# Run as root
USER root:root

# Configure the system
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
 	curl \
 	git \
 	openssh-client \
	wget \
	unzip \
	zlib1g-dev

# Install Tini
ENV TINI_VERSION v0.17.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /sbin/tini
RUN chmod +x /sbin/tini

# Install the zip extension
RUN docker-php-ext-install zip \
 && docker-php-ext-enable zip

# Add the composer installer script and the entry point script
ADD usr/local/bin/install-composer.sh /usr/local/bin/install-composer.sh
ADD usr/local/bin/docker-image-entrypoint.sh /usr/local/bin/docker-image-entrypoint

# Make them executable
RUN chmod +x /usr/local/bin/install-composer.sh \
 && chmod +x /usr/local/bin/docker-image-entrypoint

# Install Composer
RUN /usr/local/bin/install-composer.sh \
 && mv composer.phar /usr/local/bin/composer \
 && composer self-update \
 && chown php:php /usr/local/bin/composer

# Cleanup
RUN rm /usr/local/bin/install-composer.sh \
 && apt-get autoremove -y \
 && apt-get purge -y

# Run the container as the configured user
USER php:php

# Execute composer
ENTRYPOINT ["/usr/local/bin/docker-image-entrypoint"]

# The default command to execute
CMD ["composer"]
