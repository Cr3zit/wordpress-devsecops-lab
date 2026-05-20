FROM wordpress:6.4-apache

LABEL maintainer="Cr3zit"
LABEL description="Hardened WordPress image - DSO Lab 2026"

# Update all system packages
RUN apt-get update && apt-get upgrade -y \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Remove sensitive files that expose WordPress version
RUN rm -f /usr/src/wordpress/readme.html \
    && rm -f /usr/src/wordpress/license.txt \
    && rm -f /usr/src/wordpress/wp-config-sample.php

# Update Twenty Twenty-Four theme to latest (1.4)
RUN apt-get update && apt-get install -y unzip curl \
    && curl -L https://downloads.wordpress.org/theme/twentytwentyfour.1.4.zip \
       -o /tmp/twentytwentyfour.zip \
    && unzip -o /tmp/twentytwentyfour.zip -d /usr/src/wordpress/wp-content/themes/ \
    && rm /tmp/twentytwentyfour.zip \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Copy custom hardening config
COPY hardening.conf /etc/apache2/conf-enabled/hardening.conf

# Copy WordPress hardening config (disables xmlrpc, wp-cron, hides headers)
COPY wp-hardening.php /usr/src/wordpress/wp-content/mu-plugins/wp-hardening.php

# Fix permissions - run as non-root
RUN chown -R www-data:www-data /usr/src/wordpress \
    && find /usr/src/wordpress -type f -exec chmod 640 {} \; \
    && find /usr/src/wordpress -type d -exec chmod 750 {} \;

EXPOSE 80