# -*- mode: ruby -*-

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.hostname = 'docker-builder'
  config.vm.provider :digital_ocean do |provider, override|
    override.ssh.private_key_path = '~/.ssh/id_rsa'
    override.vm.box     = 'digital_ocean'
    override.vm.box_url = "https://github.com/smdahlen/vagrant-digitalocean/raw/master/box/digital_ocean.box"

    provider.image = 'Ubuntu 14.04 x64'

    provider.region = 'Singapore 1'
    provider.client_id = ENV['DO_CLIENT_ID']
    provider.api_key   = ENV['DO_API_KEY']
  end

  config.omnibus.chef_version = :latest

  config.vm.provision :shell, :inline => <<-"EOT"
  apt-get update
  apt-get install -y docker.io unzip
  wget https://dl.bintray.com/mitchellh/packer/0.6.0_linux_amd64.zip --quiet -P /tmp
  unzip /tmp/0.6.0_linux_amd64.zip -d /usr/local/bin/
  ln -s /usr/bin/docker.io /usr/local/bin/docker
  docker login -e #{ENV['DOCKER_EMAIL']} -p #{ENV['DOCKER_PASS']} -u #{ENV['DOCKER_USER']}
  EOT
end
