# Use the latest version of the Amazon Linux base image
FROM amazonlinux:2

# Update all installed packages
RUN yum update -y

# Install required packages
RUN yum install -y unzip wget httpd git

# Enable PHP 7.4
RUN amazon-linux-extras enable php7.4 && \
    yum clean metadata && \
    yum install -y php-cli php-pdo php-fpm php-json php-mysqlnd

# Install MySQL client
RUN wget https://repo.mysql.com/mysql80-community-release-el7-3.noarch.rpm && \
    rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2023 && \
    yum localinstall mysql80-community-release-el7-3.noarch.rpm -y && \
    yum install -y mysql-community-server

# Set working directory
WORKDIR /var/www/html

# Copy your local application code into the container
# Make sure you cloned your repo locally first
COPY application-codes/ /var/www/html/

# Set file permissions
RUN chmod -R 755 /var/www/html && \
    chown -R apache:apache /var/www/html

# Configure Apache
RUN sed -i '/<Directory "\/var\/www\/html">/,/<\/Directory>/ s/AllowOverride None/AllowOverride All/' /etc/httpd/conf/httpd.conf

# Expose Apache port
EXPOSE 80

# Start Apache in the foreground
CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]
