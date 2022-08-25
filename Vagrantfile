Vagrant.configure("2") do |config|
  config.vm.box = "almalinux/8"
    config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
    config.vm.provision "shell", inline: <<-SHELL
      sudo dnf -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
      sudo dnf install -y nginx createrepo
      dnf download transmission-daemon transmission-common libnatpmp
      dnf config-manager --set-disabled epel
      sudo systemctl start nginx
      sudo systemctl enable nginx
      sudo mkdir /usr/share/nginx/html/repo/
      sudo cp * /usr/share/nginx/html/repo/
      sudo createrepo /usr/share/nginx/html/repo/
      echo -en "location / {\n root /usr/share/nginx/html;\n index index.html index.htm;\n autoindex on;\n }\n\n" | sudo tee /etc/nginx/conf.d/default.conf
      echo -en "[local]\nname=Local\nbaseurl=http:///localhost/repo/\nenabled=1\ngpgcheck=0\n\n" | sudo tee /etc/yum.repos.d/local.repo
    SHELL
end
