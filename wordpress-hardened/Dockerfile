FROM wordpress:6.4-apache

LABEL maintainer="Cr3zit"
LABEL description="Hardened WordPress image - DSO Lab 2026"

# Update all system packages
RUN apt-get update && apt-get upgrade -y \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Copy hardening apache config
COPY hardening.conf /etc/apache2/conf-enabled/hardening.conf

# Copy hardening plugin to source (WordPress copies this to /var/www/html at runtime)
RUN mkdir -p /usr/src/wordpress/wp-content/mu-plugins
COPY wp-hardening.php /usr/src/wordpress/wp-content/mu-plugins/wp-hardening.php

# Remove sensitive files from source
RUN rm -f /usr/src/wordpress/readme.html \
    && rm -f /usr/src/wordpress/license.txt \
    && rm -f /usr/src/wordpress/wp-config-sample.php

EXPOSE 80