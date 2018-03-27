taken from https://mike632t.wordpress.com/2016/11/27/display-ip-address-in-banner-text/
```
nano /etc/network/if-up.d/sh-update-address
```
```
#!/bin/bash
#
# /etc/network/if-up.d/sh-update-address
#
# Checks to see if the current folder is on a file system with less than the
# specified percentage of free space and prints a warning if it is.
#
# For a description of the environment vairables  that can be sued by network
# scripts refer to 'man interfaces'.
#
# 24 Nov 16 - 0.1   - Initial version - MEJT
# 26 Jul 17 - 0.2   - Updated for Debian (stretch) - MEJT
#
# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or (at
# your option) any later version.
#
# This program is distributed in the hope that it will be useful, but
# WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU
# General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program. If not, see <http://www.gnu.org/licenses/>
#
# If '/etc/issue.std' does not exist then create if from '/etc/issue'.
if [ ! -e /etc/issue.std ]; then
  if [ -e /etc/issue ]; then mv /etc/issue /etc/issue.std; fi
fi
 
if [ "$METHOD" != loopback ]; then # Ignore the loop back adapter.
  if [ "$MODE" = start ]; then
    # Check that an IP address is defined
    if [ "$ADDRFAM" = inet ] || [ "$ADDRFAM" = inet6 ]; then
      if [ -e /etc/issue.std ]; then # Update banner text if it exists
        cp /etc/issue.std /etc/issue
        /sbin/ifconfig | grep "inet " | grep -v "127.0.0.1" | sed "s/[^0-9. ]*//g" |tr -s " " | cut -f 2 -d " " >> /etc/issue
        # /sbin/ifconfig | grep "inet addr" | grep -v "127.0.0.1" | cut -d ":" -f 2 | cut -d " " -f 1 >> 
        printf "\n" >> /etc/issue
      fi
    fi
  else
    if [ "$MODE" = stop ]; then
      if [ -e /etc/issue.std ]; then cp /etc/issue.std /etc/issue ; fi
    fi
  fi
fi
```
```
chmod +x /etc/network/if-up.d/sh-update-address
ln /etc/network/if-up.d/sh-update-address \
 /etc/network/if-down.d/sh-update-address
```
```
apt-get -y install apache2 default-mysql-server php7.2 php7.2-mysqli php-gd libapache2-mod-php libapache2-mod-security2 git
```
```
cd /etc/modsecurity/
cp modsecurity.conf-recommended modsecurity.conf
git clone https://github.com/SpiderLabs/owasp-modsecurity-crs
cd owasp-modsecurity-crs/
cp crs-setup.conf.example crs-setup.conf
vi /etc/apache2/mods-enabled/security2.conf 
```
Remove host header IP warning
```
cd /etc/modsecurity/owasp-modsecurity-crs
mkdir custom_rules
vi custom_rules/modsecurity_crs_99_custom.conf
```
```
SecRuleRemoveById 920350      
```
```
<IfModule security2_module>
        # Default Debian dir for modsecurity's persistent data
        SecDataDir /var/cache/modsecurity

        # Include all the *.conf files in /etc/modsecurity.
        # Keeping your local configuration in that directory
        # will allow for an easy upgrade of THIS file and
        # make your life easier
        IncludeOptional /etc/modsecurity/*.conf
        IncludeOptional /etc/modsecurity/owasp-modsecurity-crs/crs-setup.conf
        IncludeOptional /etc/modsecurity/owasp-modsecurity-crs/activated_rules/*conf
        # Include OWASP ModSecurity CRS rules if installed
        #IncludeOptional /usr/share/modsecurity-crs/owasp-crs.load
        SecDisableBackendCompression On
</IfModule>
```
```
cd /var
rm -rf www/
git clone https://github.com/RandomStorm/DVWA.git www
cd www
mv config/config.inc.php.dist config/config.inc.php
chmod 775 /var/www/hackable/uploads
chmod ug+w /var/www/external/phpids/0.6/lib/IDS/tmp/phpids_log.txt
chmod 775 /var/www/config
chown www-data:www-data /var/www/ -R
mysql_secure_installation
```
```
mysql -u root -p
```
create database
```
vi config/config.inc.php
```
connect to database
```
vi /etc/apache2/sites-available/000-default.conf
```
change document root
```
vi /etc/php/7.2/apache2/php.ini
```
```allow_url_include = on``` - Allows for Remote File Inclusions (RFI) [allow_url_include]
```allow_url_fopen = on``` - Allows for Remote File Inclusions (RFI) [allow_url_fopen]
```display_errors = on``` - I like to see stuff.

