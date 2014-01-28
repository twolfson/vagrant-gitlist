# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"
  config.vm.network "forwarded_port", guest: 8080, host: 8080

  # DEV: Customize your projects folder here
  config.vm.synced_folder "/home/todd/github", "/var/www/projects"

  # Update apt-get once
  $update_apt_get = <<SCRIPT
  if ! test -f .updated_apt_get; then
    sudo apt-get update
    touch .updated_apt_get
  fi
SCRIPT
  config.vm.provision "shell", inline: $update_apt_get

  # Install dependency on `git`
  $install_git = <<SCRIPT
  if ! which git &> /dev/null; then
    sudo apt-get install git -y
  fi
SCRIPT
  config.vm.provision "shell", inline: $install_git

  # Install dependency on `php`
  $install_php = <<SCRIPT
  if ! which php &> /dev/null; then
    sudo apt-get install php5-cli php5-fpm -y
  fi
SCRIPT
  config.vm.provision "shell", inline: $install_php

  # Install dependency on `nginx`
  $install_nginx = <<SCRIPT
  if ! test -d /etc/nginx/; then
    sudo apt-get install nginx -y
  fi
SCRIPT
  config.vm.provision "shell", inline: $install_nginx

  # Install dependency on `gitlist` itself
  $install_gitlist = <<SCRIPT
  if ! test -d /var/www/gitlist; then
    # Download and extract gitlist
    cd /tmp
    wget https://s3.amazonaws.com/gitlist/gitlist-0.4.0.tar.gz
    sudo tar -xzvf gitlist-0.4.0.tar.gz -C /var/www/

    # Take ownership of gitlist
    sudo chown vagrant -R /var/www/gitlist/
    cd /var/www/gitlist/
    # DEV: This must be twice escaped slashes to account for being inside of ruby
    sed 's/\\/home\\/git\\/repositories\\//\\/var\\/www\\/projects/' config.ini-example > config.ini
    sudo chown vagrant /var/www/gitlist/config.ini

    # Install cache folder
    mkdir cache
    chmod 777 cache
  fi
SCRIPT
  config.vm.provision "shell", inline: $install_gitlist

  # TODO: Prompt user to do this or somehow automate it
  # pico config.ini # repositories[] = 'path/to/linked/folders'

  # Configure nginx
  $config_nginx = <<SCRIPT
  if ! test -f /etc/nginx/conf.d/gitlist.conf; then
    # Copy over nginx config
    sudo cp /vagrant/nginx.conf /etc/nginx/conf.d/gitlist.conf

    # Restart nginx
    sudo /etc/init.d/nginx restart
  fi
SCRIPT
  config.vm.provision "shell", inline: $config_nginx
end