# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"
  config.vm.network "forwarded_port", guest: 8080, host: 8080

  # # TODO: Remove this as it is for personal development
  # config.vm.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]

  # Update apt-get once
  $update_apt_get = <<SCRIPT
  if ! test -f .updated_apt_get; then
    sudo apt-get update
    touch .updated_apt_get
  fi
SCRIPT
  config.vm.provision "shell", inline: $update_apt_get

  # Install test dependency on `git`
  $install_git = <<SCRIPT
  if ! which git &> /dev/null; then
    sudo apt-get install git -y
  fi
SCRIPT
  config.vm.provision "shell", inline: $install_git

  # DEV: Customize your projects folder here
  config.vm.synced_folder "/home/todd/github", "/var/www/projects"

  # TODO: Install test dependency on `php`
  # sudo apt-get install php5-cli php5-fpm

  # TODO: Install test dependency on `nginx`
  # sudo apt-get install nginx

  # TODO: Install test dependency on `gitlist` itself
  # wget https://s3.amazonaws.com/gitlist/gitlist-0.4.0.tar.gz
  # sudo tar -xzvf gitlist-0.4.0.tar.gz -C /var/www/
  # sudo chown $USER -R /var/www/gitlist/
  # cd /var/www/gitlist/
  # cp config.ini-example config.ini
  # pico config.ini # repositories[] = 'path/to/linked/folders'

  # mkdir cache
  # chmod 777 cache

  # Copy over nginx conf from https://github.com/klaussilveira/gitlist/blob/master/INSTALL.md#nginx-serverconf
  # listen 8080;
  # server_name localhost;
  # # access_log /var/log/nginx/gitlist.access_log main;
  # access_log /var/log/nginx/gitlist.access_log;
  # error_log /var/log/nginx/gitlist.error_log debug_http;

  # root /var/www/gitlist;
  # index index.php;

  # ...
  #  #       include fastcgi.conf;
  #      include fastcgi_params;

  # Set up nginx
  # sudo mkdir -p /var/log/nginx/
  # sudo /etc/init.d/nginx restart
end