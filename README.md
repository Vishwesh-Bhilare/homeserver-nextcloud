Here's a version of the **Nextcloud Setup** using GitHub Markdown syntax that you can paste directly into a README file on GitHub:

```markdown
# Nextcloud Installation Guide (Ubuntu 22.04)

## Prerequisites
- Ubuntu 22.04 server
- Domain name
- 2GB+ RAM recommended
- SSH access

## Step 1: Update System

```bash
sudo apt update && sudo apt upgrade -y
```

## Step 2: Install LAMP Stack

### Apache:
```bash
sudo apt install apache2 -y
```

### MariaDB:
```bash
sudo apt install mariadb-server -y
sudo mysql_secure_installation
```

### PHP:
```bash
sudo apt install php php-mysql php-xml php-gd php-curl php-zip php-mbstring -y
```

## Step 3: Configure MariaDB

```bash
sudo mysql -u root -p
```

```sql
CREATE DATABASE nextcloud;
CREATE USER 'nextclouduser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON nextcloud.* TO 'nextclouduser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

## Step 4: Install Nextcloud

1. Download and unzip Nextcloud:
    ```bash
    cd /var/www/
    sudo wget https://download.nextcloud.com/server/releases/nextcloud-25.0.0.zip
    sudo unzip nextcloud-25.0.0.zip
    ```

2. Set permissions:
    ```bash
    sudo chown -R www-data:www-data /var/www/nextcloud
    sudo chmod -R 755 /var/www/nextcloud
    ```

## Step 5: Configure Apache

1. Create a new config file:
    ```bash
    sudo nano /etc/apache2/sites-available/nextcloud.conf
    ```

2. Paste the following:
    ```apache
    <VirtualHost *:80>
        DocumentRoot "/var/www/nextcloud/"
        ServerName yourdomain.com

        <Directory /var/www/nextcloud/>
            Require all granted
            AllowOverride All
            Options FollowSymLinks MultiViews
        </Directory>
    </VirtualHost>
    ```

3. Enable the config and restart Apache:
    ```bash
    sudo a2ensite nextcloud.conf
    sudo a2enmod rewrite headers env dir mime
    sudo systemctl restart apache2
    ```

## Step 6: Enable SSL

1. Install Certbot:
    ```bash
    sudo apt install certbot python3-certbot-apache -y
    ```

2. Obtain and install SSL:
    ```bash
    sudo certbot --apache -d yourdomain.com
    ```

## Step 7: Complete Setup in Browser

Navigate to `http://yourdomain.com` and follow the web installer.

- **Database User**: `nextclouduser`
- **Database Password**: `password`
- **Database Name**: `nextcloud`
- **Database Host**: `localhost`

## Optional: Redis Caching

1. Install Redis and PHP Redis module:
    ```bash
    sudo apt install redis-server php-redis
    ```

2. Enable Redis:
    ```bash
    sudo systemctl enable redis-server
    ```

## Troubleshooting

Check logs for issues:

```bash
sudo journalctl -xe
```

## References
- [Nextcloud Documentation](https://docs.nextcloud.com)
```

This document uses GitHub Markdown syntax and includes code blocks, making it easy to paste directly into a GitHub repository as a `README.md` file.
