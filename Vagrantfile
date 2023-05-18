Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-22.04"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 4000
    vb.cpus = 2
  end
  config.vm.network "private_network", ip: "192.168.70.10"
  config.vm.network "public_network"
  config.vm.provision "shell", inline: <<-SHELL
    # update
    echo ".........----------------#################._.-.-UPDATING UBUNTU 22.04 LTS-.-._.#################----------------........."
    apt-get update -y
    apt-get install ca-certificates curl -y
    update-ca-certificates

    # install & start jenkins
    echo ".........----------------#################._.-.-INSTALLING JENKINS-.-._.#################----------------........."
    curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
        /usr/share/keyrings/jenkins-keyring.asc > /dev/null
    echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
        https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
        /etc/apt/sources.list.d/jenkins.list > /dev/null
    apt-get update -y
    apt-get install fontconfig openjdk-11-jre -y
    apt-get install jenkins -y
    systemctl start jenkins
    systemctl enable jenkins
    # Move files to install extra jenkins plugins
    cp /vagrant/plugins.txt .
    cp /vagrant/plugins-installer.sh .

    # Install & start Docker
    echo ".........----------------#################._.-.-INSTALLING DOCKER-.-._.#################----------------........."
    apt-get update -y
    apt-get install apt-transport-https ca-certificates curl software-properties-common -y
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    apt-get update -y
    apt-cache policy docker-ce -y
    apt-get install docker-ce -y
    usermod -aG docker vagrant
    su - vagrant
    groups
    chmod 666 /var/run/docker.sock

    # Install git, maven, jq
    echo ".........----------------#################._.-.-INSTALLING GIT, MAVEN, JQ-.-._.#################----------------........."
    apt-get install git maven jq -y

    # Install Kubernetes (microk8s)
    echo ".........----------------#################._.-.-INSTALLING KUBERNETES (MIKROK8S)-.-._.#################----------------........."
    apt-get update -y
    snap install kubectl --classic
    snap install microk8s --classic
    usermod -a -G microk8s vagrant
    chown -f -R vagrant ~/.kube

    # remove unnecessary packages
    echo ".........----------------#################._.-.-UNINSTALLING UNNECESSARY DEPENDENCIES-.-._.#################----------------........."
    apt-get autoremove -y
  SHELL
end