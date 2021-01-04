# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.network "forwarded_port", guest: 3000, host: 3000

  config.vm.provision "shell", privileged: false, inline: <<-SHELL
  sudo apt-get update

  command curl -sSL https://rvm.io/mpapis.asc | gpg --import -
  command curl -sSL https://rvm.io/pkuczynski.asc | gpg --import -

  curl -sSL https://get.rvm.io | bash -s stable
  source $HOME/.rvm/scripts/rvm
  rvm use --default --install 2.7.2

  gem install bundler
  gem install rails

  cat >> /home/vagrant/upgrade_distinct.sh <<'EOL'
#!/bin/bash
sudo apt-get update -y
sudo apt-get upgrade -y
sudo apt-get dist-upgrade -y
EOL
  chmod +x /home/vagrant/upgrade_distinct.sh

  /home/vagrant/upgrade_distinct.sh

  cat >> /home/vagrant/install_postgres.sh <<'EOL'
#!/bin/bash
sudo apt-get install -y postgresql libpq-dev
sudo -u postgres createdb 'default_db'
EOL
  chmod +x /home/vagrant/install_postgres.sh

  /home/vagrant/install_postgres.sh

  echo "New Ruby on Rails VM with PostgreSQL successfully created!"
SHELL
end
