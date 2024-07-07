Vagrant.configure("2") do |config|
  config.vm.define "sonar" do |ubuntu1|
    ubuntu1.vm.box = "ubuntu/jammy64"
    # Create a private network
    ubuntu1.vm.network "private_network", type: "dhcp"
    # Hostname
    ubuntu1.vm.hostname = "sonar.com"

    # Provider-specific configuration
    ubuntu1.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
      vb.cpus = "2"
      vb.name = "sonar"
    end

    # Provisioning script
    ubuntu1.vm.provision "shell", inline: <<-SHELL
      # Update and install required packages
      apt-get update
      apt-get install ca-certificates curl
      install -m 0755 -d /etc/apt/keyrings

      # Add Docker's official GPG key
      curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
      chmod a+r /etc/apt/keyrings/docker.asc

      # Add Docker's repository
      echo \
       "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
       $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
       tee /etc/apt/sources.list.d/docker.list > /dev/null

      # Update and install Docker
      apt-get update
      apt-get install -y docker-ce

      # Create user named 'docker' and add to the 'docker' group
      useradd -m -s /bin/bash -g docker docker

      # Set the password for the 'docker' user
      echo "docker:password" | chpasswd

      # set docker in sudo group
      usermod -aG sudo docker

      # Verify Docker installation
      docker --version
    SHELL
  end
end

