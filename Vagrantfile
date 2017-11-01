# -*- mode: ruby -*-
# vi: set ft=ruby :
#
# Vagrant box with docker-ce and docker-compose
#
# https://docs.docker.com/engine/installation/linux/docker-ce/debian/#install-using-the-repository
# https://docs.docker.com/compose/install/#install-compose
#

DOCKER_VERSION = "17.09.0~ce-0~debian"
COMPOSE_VERSION = "1.16.1"


$script = <<SCRIPT

export DEBIAN_FRONTEND=noninteractive

apt-get update && apt-get install -y --no-install-recommends \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg2 \
    software-properties-common

echo "Installing docker via apt repo..."
curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg | sudo apt-key add -
apt-key fingerprint 0EBFCD88

add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") \
    $(lsb_release -cs) \
    stable"

apt-get update && apt-get install -y --no-install-recommends \
    docker-ce=#{DOCKER_VERSION}

echo "Installing docker-compose from source..."
curl -fsSL https://github.com/docker/compose/releases/download/#{COMPOSE_VERSION}/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose

echo "Adding vagrant user to docker and adm groups..."
groupadd docker &> /dev/null
usermod -aG docker vagrant
usermod -aG adm vagrant

echo "Writing docker aliases..."
cat > /etc/profile.d/00-aliases.sh <<EOF
alias d="docker"
alias dc="docker-compose"
EOF

echo "Enojy! :)"
SCRIPT

def get_ipaddr(hostname, default)
  return Socket::getaddrinfo(hostname, nil)[0][3]
  rescue SocketError
    return default
end

Vagrant.configure("2") do |config|
    config.vm.box = "debian/jessie64"
    config.vm.hostname = "jessie-docker"

    private_network_ip = get_ipaddr(config.vm.hostname, "10.10.10.10")
    config.vm.network "private_network", ip: private_network_ip

    config.vm.provision "shell", inline: $script

    config.vm.post_up_message = \
      "The private network IP address is: #{private_network_ip}\n\n" \
      "To customize, set the host called '#{config.vm.hostname}'\n" \
      "to the desired IP address in your /etc/hosts and run \n" \
      "'vagrant reload'!\n"
end
