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

  cat >> /home/vagrant/install_libs.sh <<'EOL'
#!/bin/bash
sudo apt-get update -y
sudo apt-get upgrade -y
sudo apt-get dist-upgrade -y
sudo apt-get install -y zip mlocate
EOL
  chmod +x /home/vagrant/install_libs.sh

  /home/vagrant/install_libs.sh

  cat >> /home/vagrant/install_postgres.sh <<'EOL'
#!/bin/bash
sudo apt-get install -y postgresql libpq-dev
EOL
  chmod +x /home/vagrant/install_postgres.sh

  /home/vagrant/install_postgres.sh

  cat >> /home/vagrant/install_node_and_yarn.sh <<'EOL'
#!/bin/bash
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
sudo apt-get install -y nodejs

curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update -y
sudo apt-get install -y yarn
yarn set version latest
EOL
  chmod +x /home/vagrant/install_node_and_yarn.sh

  /home/vagrant/install_node_and_yarn.sh

  cat >> /home/vagrant/.bashrc <<EOL
cd /vagrant
EOL

  echo "New Ruby on Rails VM with PostgreSQL successfully created!"
SHELL
end
