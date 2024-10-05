Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.boot_timeout = 600
  config.vm.synced_folder ".", "/vagrant", disabled: false
  config.vm.network "forwarded_port", guest: 22, host: 2222

  # Configure a rede com IP fixo ao invés de DHCP
  config.vm.network "private_network", type: "static", ip: "192.168.50.4"

  config.vm.provision "shell", inline: <<-SHELL
    # Atualizando pacotes e instalando dependências
    sudo apt-get update
    sudo apt-get install -y docker.io wget unzip

    # Adicionando usuário vagrant ao grupo docker
    sudo usermod -aG docker vagrant

    # Baixando e instalando Terraform
    wget https://releases.hashicorp.com/terraform/1.5.0/terraform_1.5.0_linux_amd64.zip
    unzip terraform_1.5.0_linux_amd64.zip
    sudo mv terraform /usr/local/bin/

    # Executando script da pasta compartilhada
    chmod +x /vagrant/script.sh
    /vagrant/script.sh
  SHELL

  # Redirecionando porta do host para acessar o container NGINX
  config.vm.network "forwarded_port", guest: 80, host: 8080
end


