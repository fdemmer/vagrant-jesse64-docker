# install docker-ce and docker-compose
#
# https://docs.docker.com/engine/installation/linux/docker-ce/debian/#install-using-the-repository
# https://docs.docker.com/compose/install/#install-compose

$script = <<SCRIPT

apt-get update && apt-get install -y --no-install-recommends \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg2 \
    software-properties-common

curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg | sudo apt-key add -
apt-key fingerprint 0EBFCD88

add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") \
    $(lsb_release -cs) \
    stable"

apt-get update && apt-get install -y --no-install-recommends \
    docker-ce

curl -fsSL https://github.com/docker/compose/releases/download/1.16.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose

groupadd docker &> /dev/null
usermod -aG docker vagrant

SCRIPT

Vagrant.configure("2") do |config|
    config.vm.box = "debian/jessie64"
    config.vm.provision "shell", inline: $script
end
