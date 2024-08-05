# Configuring a GLPI server on Debian for ticket management
---
## Installation prerequisites

**1. Check for updates and install them**
 
   `sudo apt-get update && sudo apt-get upgrade`
   
**2. Install Apache, which will host the GLPI server, and enable it**

   `sudo apt-get install apache2 -y`
   
   `sudo systemctl enable apache2`
   
**3. Install MariaDB, which will serve as the database for GLPI**

   `sudo apt-get install mariadb-server -y`
   
**4. Install PHP module and its annexes**

   `sudo apt-get install php libapache2-mod-php -y`
   
**5. Check PHP version (7.4 to 8.2.0 to be compatible with GLPI)**

   `sudo php --version`

   ---
   
   If your version is not between 7.4 and 8.2.0, it needs to be modified.
   
   Versions prior to 8.3 are not native to Debian, so a specific library needs to be installed to retrieve them.
   
   a. *Modify the *`/etc/apt/sources.list`* file to add a new package library*
   
   > deb https://packages.sury.org/php bookworm main

   b. *Update packages*
   
   `sudo apt-get update && sudo apt-get upgrade`
   
   c. *Install PHP 7.4 or 8.1*
   
   `sudo apt install php7.4` or `sudo apt install php8.1`
   
   d. *Modify the PHP version used (as well as phar and phar.phar) by selecting the desired versions*
   
   `sudo update-alternatives --config php` / `sudo update-alternatives --config phar` / `sudo update-alternatives --config phar.phar`
   
   e. *Change the PHP version used by Apache*
   
   `sudo a2dismod php8.3`
   
   `sudo a2enmod php7.4`
   
   f. *Restart Apache*
   
   `sudo systemctl restart apache2`

   g. *Install PHP annexes modules for a specific version*

  `sudo apt-get install php7.4-{ldap,imap,apcu,xmlrpc,curl,common,gd,json,mbstring,mysql,xml,intl,zip,bz2}`

  ---
  
**7. Create the database with Maria DB**

   ```bash
   # Initialize the database
   sudo mysql_secure_installation
   # Connect to the database
   mysql -u root -p
   # Create the database (modify databaseName, accountName and password)
   create database "databaseName" character set utf8 collate utf8_bin;
   grant all privileges on "databaseName".* to "accountName"@localhost identified by 'password';
   flush privileges;
   quit
   ```

**8. Retrieve GLPI sources**
   ```bash
   sudo wget https://github.com/glpi-project/glpi/releases/download/10.0.2/glpi-10.0.2.tgz
   # Replace "myDomainName" with your domain or create one
   sudo mkdir /var/www/html/glpi."myDomainName"
   sudo tar -xzvf glpi-10.0.2.tgz
   sudo cp -R glpi/* /var/www/html/glpi."myDomainName"/
   sudo chown -R www-data:www-data /var/www/html/glpi."myDomainName"/
   sudo chmod -R 775 /var/www/html/glpi."myDomainName"/
   ```

## Configuring GLPI

**1. Connect to the GLPI server interface from a client**

From a domain client, access GLPI via a browser with the URL **http://serverIpAddress/glpi."myDomainName"**
