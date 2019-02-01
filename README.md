# Computer Setup
## ZSH
```
sudo apt install zsh
sudo chsh -s $(which zsh)
```
### Oh my zsh
see https://ohmyz.sh/ for more information
```
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
#### Spaceship-Theme
```
git clone https://github.com/denysdovhan/spaceship-prompt.git "$ZSH_CUSTOM/themes/spaceship-prompt"
ln -s "$ZSH_CUSTOM/themes/spaceship-prompt/spaceship.zsh-theme" "$ZSH_CUSTOM/themes/spaceship.zsh-theme"
```
## LAMP
### Apache
```
sudo apt install apache2
```
#### VirtualHosts
##### HTTP
```
<VirtualHost *:80>
    ServerName DOMAIN.localhost
    DocumentRoot /PATH/TO/YOUR/DOCUMENT_ROOT

	<Directory /PATH/TO/YOUR/DOCUMENT_ROOT>
		Options +FollowSymlinks
        AllowOverride All
        Require all granted
	</Directory>

    <FilesMatch \.php$>
		SetHandler "proxy:unix:/var/run/php/php7.2-fpm.sock|fcgi://localhost/"
    </FilesMatch>
</VirtualHost>
```
##### HTTP to HTTPS
```
<VirtualHost *:80>
    ServerName DOMAIN.localhost
    RedirectMatch 301 ^(.*)$ https://DOMAIN.localhost$1
</VirtualHost>
```
##### HTTPS
```
<VirtualHost *:443>
    ServerName DOMAIN.localhost
    DocumentRoot /PATH/TO/YOUR/DOCUMENT_ROOT

	<Directory /PATH/TO/YOUR/DOCUMENT_ROOT>
		Options +FollowSymlinks
        AllowOverride All
        Require all granted
	</Directory>

    <FilesMatch \.php$>
		SetHandler "proxy:unix:/var/run/php/php7.2-fpm.sock|fcgi://localhost/"
    </FilesMatch>

	SSLEngine on
	SSLCertificateFile /PATH/TO/YOUR/CERT_ROOT/cert.pem
	SSLCertificateKeyFile /PATH/TO/YOUR/CERT_ROOT/key.pem
</VirtualHost>
```
### MySQL
```
sudo apt install mysql-server
```
### PHP
```
sudo apt install php php-fpm php-pdo php-mysql ...
```
#### Set PHP-FPM User
```
sudo nano /etc/php/7.2/fpm/pool.d/www.conf
```
At `; Unix user/group of processes` set the user to your user: `user = www-data` -> `user = YOUR_USERNAME`. Save and restart PHP-FPM and Apache2.
## NVM
```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | zsh
nvm install node
```
## GO
Download latest version of GO from https://golang.org/dl/. After downloading run:
```
sudo tar -C /usr/local -xzf go1.11.5.linux-amd64.tar.gz
```
and add GO to your PATH:
```
export PATH=$PATH:/usr/local/go/bin
```
### minica (for HTTPS)
see https://github.com/jsha/minica for more information
```
cd /ANY/PATH
git clone github.com/jsha/minica
go build
```
#### Usage
``` 
minica --domains DOMAIN.localhost
```
### mhsendmail (for PHP mailing)
see https://github.com/mailhog/mhsendmail for more information
```
go get github.com/mailhog/mhsendmail
mhsendmail test@mailhog.local <<EOF
From: App <app@mailhog.local>
To: Test <test@mailhog.local>
Subject: Test message

Some content!
EOF
```
#### Usage (php.ini)
```
sendmail_path = /usr/local/bin/mhsendmail
```