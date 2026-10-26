Vagrant.configure("2") do |config|
  # obraz bazowy - Ubuntu 24.04
  config.vm.box = "bento/ubuntu-24.04"
  config.vm.hostname = "lab-web"

  # siec prywatna ze stalym adresem (jak host-only)
  config.vm.network "private_network", ip: "192.168.56.10"

  # port 8081 na moim komputerze -> port 80 w maszynie, tylko z localhost
  # (8080 u mnie zajety przez Apache)
  config.vm.network "forwarded_port", guest: 80, host: 8081, host_ip: "127.0.0.1"

  config.vm.provider "virtualbox" do |vb|
    vb.name = "lab-web"
    vb.memory = 1024
    # 1 procesor - z 2 maszyna u mnie zawieszala sie przy starcie
    vb.cpus = 1
  end

  # wykonuje sie raz, przy pierwszym vagrant up
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y nginx
    echo '<meta charset="utf-8"><h1>Maciej - lab-web z Vagranta</h1>' > /var/www/html/index.html
  SHELL
end
