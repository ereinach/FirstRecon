# -*- mode: ruby -*-
# vi: set ft=ruby :
#
# WPDistillery Vagrantfile using Scotch Box
# Check out https://box.scotch.io to learn more about Scotch Box
#
# File Version: 1.2.1


VAGRANTFILE_API_VERSION = '2'

@script = <<SCRIPT
# Fix for https://bugs.launchpad.net/ubuntu/+source/livecd-rootfs/+bug/1561250
if ! grep -q "ubuntu-xenial" /etc/hosts; then
    echo "127.0.0.1 ubuntu-xenial" >> /etc/hosts
fi
# Install dependencies
add-apt-repository ppa:ondrej/php
apt-get update
apt-get install -y apache2 git curl php7.2 php7.2-bcmath php7.2-bz2 php7.2-cli php7.2-curl php7.2-intl php7.2-json php7.2-mbstring php7.2-opcache php7.2-soap php7.2-sqlite3 php7.2-xml php7.2-xsl php7.2-zip libapache2-mod-php7.2
# Configure Apache
echo "<VirtualHost *:80>
    DocumentRoot /var/www/public
    AllowEncodedSlashes On
    <Directory /var/www/public>
        Options +Indexes +FollowSymLinks
        DirectoryIndex index.php index.html
        Order allow,deny
        Allow from all
        AllowOverride All
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>" > /etc/apache2/sites-available/000-default.conf
a2enmod rewrite
service apache2 restart
if [ -e /usr/local/bin/composer ]; then
    /usr/local/bin/composer self-update
else
    curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
fi
# Reset home directory of vagrant user
if ! grep -q "cd /var/www" /home/ubuntu/.profile; then
    echo "cd /var/www" >> /home/ubuntu/.profile
fi
echo "** [PHP] Run the following command to install dependencies, if you have not already:"
echo "    vagrant ssh -c 'composer install'"
echo "** [PHP] Visit http://localhost:8080 in your browser for to view the application **"
SCRIPT

Vagrant.configure("2") do |config|

    config.ssh.username = "vagrant"
    config.ssh.password = "vagrant"
    config.vm.box = "scotch/box"
    config.vm.network "private_network", ip: "192.168.33.10"
    config.vm.hostname = "p3p.vm"

    # Use vagrant-winnfsd if available https://github.com/flurinduerst/WPDistillery/issues/78
    if Vagrant.has_plugin? 'vagrant-winnfsd'
      config.vm.synced_folder ".", "/var/www",
        nfs: true,
        mount_options: [
        'nfsvers=3',
        'vers=3',
        'actimeo=1',
        'rsize=8192',
        'wsize=8192',
        'timeo=14'
        ]
    else
      config.vm.synced_folder ".", "/var/www", :mount_options => ["dmode=777", "fmode=666"]
    end

    # WPDistillery Windows Support
    if Vagrant::Util::Platform.windows?
      config.vm.provision "shell",
      inline: "echo \"Converting Files for Windows\" && sudo apt-get install -y dos2unix && cd /var/www/ && dos2unix wpdistillery/config.yml && dos2unix wpdistillery/provision.sh && dos2unix wpdistillery/wpdistillery.sh",
      run: "always", privileged: false
    end

    # Run Provisioning – executed within the first `vagrant up` and every `vagrant provision`
    config.vm.provision "shell", path: "wpdistillery/provision.sh"

    # OPTIONAL - Update WordPress and all Plugins on vagrant up – executed within every `vagrant up`
    #config.vm.provision "shell", inline: "echo \"== Update WordPress & Plugins ==\" && cd /var/www/public && wp core update && wp plugin update --all", run: "always", privileged: false

    # OPTIONAL - Enable NFS. Make sure to remove line 13 (See https://stefanwrobel.com/how-to-make-vagrant-performance-not-suck)
    #config.vm.synced_folder ".", "/var/www", :nfs => { :mount_options => ["dmode=777","fmode=666"] }

end
