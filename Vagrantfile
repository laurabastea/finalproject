# -*- mode: ruby -*-
# vi: set ft=ruby :
$setup_script = <<SCRIPT
echo "Updating the system"
apt update && apt upgrade -y
echo "install and setup docker"
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
add-apt-repository -y "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
apt-get update
apt-cache policy docker-ce
apt-get install -y docker-ce
systemctl enable docker
systemctl start docker
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  
  config.vm.define "jenkins-server" do |js|
    js.vm.hostname = "jenkins-server"
    js.vm.network "forwarded_port", guest: 8080, host: 80
    js.vm.network "private_network", ip: "10.11.11.11"
    js.vm.provision :shell, :inline => $setup_script
    js.vm.provision "shell" do |d|
      d.inline = "docker container rm -f laurabastea-jenkins"
    end
    js.vm.provision "docker" do |d|
      d.build_image "/vagrant/Jenkins" " -t laurabastea/jenkins"
      d.run "laurabastea/jenkins",
        args: " -v /vagrant/Jenkins:/jenkins -p 80:8080"
    end
    js.vm.provision :docker_login, username: "laurabastea", password: "0076b18d-2538-4014-b9c7-4ca4c77b409d"
    js.vm.provision "shell" do |d|
      d.inline = "docker image push --all-tags laurabastea/jenkins"
    end
  end

  # config.vm.define "second-server-drupal" do |sds|
  #   sds.vm.hostname = "second-server-drupal"
  #   sds.vm.network "private_network", ip: "10.11.11.12"
  #   sds.vm.provision :shell, :inline => $setup_script
  #   sds.vm.provision "docker" do |d|
  #     d.build_image "/vagrant/Drupal"
  #   end
  # end
end