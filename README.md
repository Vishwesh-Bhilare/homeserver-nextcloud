# Guide to Building a Nextcloud Server on Ubuntu 22.04

## Prerequisites
- A server running Ubuntu 22.04.
- A domain name (optional but recommended for SSL).
- Basic knowledge of Linux command line.

## Step 1: Update the System
First, ensure your system is up to date:

```bash
sudo apt update && sudo apt upgrade -y
```

## Step 2: Install Apache Web Server
Next, install Apache:

```bash
sudo apt install apache2 -y
```

Enable and start Apache:

```bash
sudo systemctl enable apache2
sudo systemctl start apache2
```

## Step 3: Install PHP and Required Extensions
Install PHP and necessary extensions:

```bash
sudo apt install php libapache2-mod-php php-mysql php-gd php-xml php-mbstring php-curl php-zip php-intl php-bcmath php-gmp -y
```

## Step 4: Install MariaDB Database Server
Install MariaDB:

```bash
sudo apt install mariadb-server -y
```

Secure the installation:

```bash
sudo mysql_secure_installation
```

## Step 5: Create Database and User for Nextcloud
Log into MariaDB:

```bash
sudo mysql -u root -p
```

Create the database and user:

```sql
CREATE DATABASE nextcloud;
CREATE USER 'nextclouduser'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON nextcloud.* TO 'nextclouduser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

## Step 6: Download and Install Nextcloud
Download Nextcloud:

```bash
cd /tmp
wget https://download.nextcloud.com/server/releases/nextcloud-24.0.0.zip
unzip nextcloud-24.0.0.zip
sudo mv nextcloud /var/www/html/
```

Set the correct permissions:

```bash
sudo chown -R www-data:www-data /var/www/html/nextcloud
sudo chmod -R 755 /var/www/html/nextcloud
```

## Step 7: Configure Apache for Nextcloud
Create a new Apache configuration file:

```bash
sudo nano /etc/apache2/sites-available/nextcloud.conf
```

Add the following configuration:

```apache
<VirtualHost *:80>
    ServerAdmin admin@example.com
    DocumentRoot /var/www/html/nextcloud/
    ServerName your_domain.com

    <Directory /var/www/html/nextcloud/>
        Options +FollowSymlinks
        AllowOverride All

        <IfModule mod_dav.c>
            Dav off
        </IfModule>

        SetEnv HOME /var/www/html/nextcloud
        SetEnv HTTP_HOME /var/www/html/nextcloud
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Enable the new configuration and required modules:

```bash
sudo a2ensite nextcloud.conf
sudo a2enmod rewrite headers env dir mime
sudo systemctl restart apache2
```

## Step 8: Install SSL Certificate (Optional)
If you have a domain, you can secure your Nextcloud with Let’s Encrypt:

```bash
sudo apt install certbot python3-certbot-apache -y
sudo certbot --apache -d your_domain.com
```

## Step 9: Complete the Installation via Web Interface
Open your web browser and navigate to `http://your_domain.com` or `http://your_server_ip`. Follow the on-screen instructions to complete the setup:
- Enter the admin account details.
- Provide the database details created earlier.
- Finish the setup and start using Nextcloud!

## Additional Tips
- Regularly update your Nextcloud instance for security and new features.
- Consider setting up a firewall to protect your server.
- Backup your data regularly.
```

Feel free to adjust any sections or content as necessary!
