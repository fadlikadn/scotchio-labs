# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

    config.vm.box = "scotch/box"
    config.vm.network "private_network", ip: "192.168.33.10"
    config.vm.hostname = "scotchbox"
    # config.vm.synced_folder ".", "/var/www", :mount_options => ["dmode=777", "fmode=666"]
    config.vm.synced_folder ".", "/var/www", :nfs => { :mount_options => ["dmode=777", "fmode=666"] }

    # username & password
    config.ssh.username = "vagrant";
    config.ssh.password = "vagrant";

    # provision
    config.vm.provision "shell", inline: <<-SHELL
    	# Shell commands here
    	# echo "Hello, I will be printed on screen during Vagrant up"

    	# update & uprade tools inside VM
    	# sudo apt-get update
    	# sudo apt-get upgrade -y

    	# creating environment variables
    	# sudo bash -c \'echo export APP_ENV="development" >> /etc/apache2/envvars\'
    	# sudo bash -c \'echo export DB_PASS="meowmeowmeow" >> /etc/apache2/envvars\'

    	# installing Xdebug
    	sudo apt-get install php5-xdebug

    	## Only thing you probably really care about is right here
    	DOMAINS=("site1.local" "site2.local" "site3.local")

    	## Loop through all sites
    	for ((i=0; i < ${#DOMAINS[@]}; i++)); do

    		## Current Domain
    		DOMAIN=${DOMAINS[$i]}

    		echo "Creating directory for $DOMAIN..."
    		mkdir -p /var/www/$DOMAIN/public

    		echo "Creating vhost config for $DOMAIN..."
    		sudo cp /etc/apache2/sites-available/scotchbox.local.conf /etc/apache2/sites-available/$DOMAIN.conf

    		echo "Updating vhost config for $DOMAIN..."
    		sudo sed -i s, scotchbox.local,$DOMAIN,g /etc/apache2/sites-available/$DOMAIN.conf
    		sudo sed -i s,/var/www/public,/var/www/$DOMAIN/public,g /etc/apache2/sites-available/$DOMAIN.conf

    		echo "Enabling $DOMAIN. Will probably tell you restart Apache..."
    		sudo a2ensite $DOMAIN.conf

    		echo "So let's restart apache..."
    		sudo service apache2 restart
    	done
SHELL
    
    # Optional NFS. Make sure to remove other synced_folder line too
    #config.vm.synced_folder ".", "/var/www", :nfs => { :mount_options => ["dmode=777","fmode=666"] }

end
