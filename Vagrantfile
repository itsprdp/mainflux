# -*- mode: ruby -*-
# vi: set ft=ruby :
# Customize these global variables
$GOOGLE_PROJECT_ID = `cat secrets/gcp_project_id`.strip
$GOOGLE_JSON_KEY_LOCATION = "secrets/gcp-service-account-key.json"
$LOCAL_USER = `whoami`.strip
$LOCAL_SSH_KEY = "~/.ssh/google_compute_engine"

Vagrant.configure("2") do |config|
  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update -qq
    apt-get install -qq -o=Dpkg::Use-Pty=0 -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common vim git make
    curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
    add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
    apt-get update -qq
    apt-get install -qq -o=Dpkg::Use-Pty=0 -y docker-ce docker-ce-cli containerd.io
    curl -L -s "https://github.com/docker/compose/releases/download/1.25.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    chmod +x /usr/local/bin/docker-compose
    ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
    cd /vagrant && make runbg
  SHELL

  # run mainflux test instance on the GCE instance
  config.vm.box = "google/gce"

  config.vm.provider :google do |google, override|
    google.google_project_id = $GOOGLE_PROJECT_ID
    google.google_json_key_location = $GOOGLE_JSON_KEY_LOCATION

    # Override provider defaults
    google.name = "mainflux-test"
    google.image = "debian-9-stretch-v20200309"
    google.image_project_id = "debian-cloud"
    google.machine_type = "n1-standard-1"
    google.zone = "us-central1-f"
    google.metadata = {'custom' => 'metadata', 'testing' => 'mainflux'}
    google.tags = ['vagrantbox', 'dev', 'http-server', 'https-server']

    override.ssh.username = $LOCAL_USER
    override.ssh.private_key_path = $LOCAL_SSH_KEY
  end
end
