"# LoingWithoutSSLPHP" 
# AWS EC2 User data for Apache and PHP Installation
#!/bin/bash 

yum update -y

amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2

yum install -y httpd mariadb-server

systemctl start httpd

systemctl enable httpd

usermod -a -G apache ec2-user

chown -R ec2-user:apache /var/www

chmod 2775 /var/www

find /var/www -type d -exec chmod 2775 {} ;

find /var/www -type f -exec chmod 0664 {} ;

echo "" > /var/www/html/phpinfo.php

# Go to apache home directory
cd /var/www/html

# Install git in your EC2 instance
sudo yum install git -y

# Clone the PHP simple login application using the following command
git clone https://github.com/codersage-in/LoingWithoutSSLPHP.git

# Create a self signed certificate using the following command:
openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:4096 -keyout localhost.key -out localhost.crt

# Now add TLS support by installing the Apache module mod_ssl on EC2 instance.
sudo yum install -y mod_ssl

# Copy the certificate and privite key file in location mentioned in /etc/httpd/conf.d/ssl.conf as SSLCertificateFile and SSLCertificateKeyFile parameters

sudo cp localhost.crt /etc/pki/tls/certs/

sudo cp localhost.key /etc/pki/tls/private/

# Read the complet tutorial on AWS Docs
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/SSL-on-amazon-linux-2.html
