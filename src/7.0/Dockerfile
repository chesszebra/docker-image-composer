FROM chesszebra/php:7.0-cli

# Run as root
USER root:root

# Configure the system
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
 	curl \
 	git \
 	openssh-client \
	wget

# Add the composer installer script
ADD usr/local/bin/install-composer.sh /usr/local/bin/install-composer.sh

# Install Composer
RUN chmod +x /usr/local/bin/install-composer.sh \
 && /usr/local/bin/install-composer.sh \
 && mv composer.phar /usr/local/bin/composer \
 && composer self-update \
 && chown php:php /usr/local/bin/composer \
 && rm /usr/local/bin/install-composer.sh

# Run the container as the configured user
USER php:php

# Execute composer
ENTRYPOINT ["composer"]
