# Distribution
FROM drupal:7.61

# Non-interactive
ENV DEBIAN_FRONTEND noninteractive

# Packages
RUN apt-get update -y && apt-get install -y ssl-cert mysql-client

# Add SSL / HTTPS support
ADD 000-default.conf /etc/apache2/sites-available/000-default.conf
RUN a2enmod rewrite ssl

# Clean Apache rights
RUN usermod -u 1000 www-data

# Expose ports
EXPOSE 443
