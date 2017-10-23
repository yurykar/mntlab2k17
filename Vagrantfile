# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
    
  (1..2).each do |i|
    config.vm.define "node-#{i}" do |node|
       node.vm.box = "sbeliakou/centos-7.4-x86_64-minimal"
       node.vm.hostname = "node-#{i}"
       node.vm.network 'private_network', ip: "172.16.1.#{i+20}"
       node.ssh.insert_key = false
       node.vm.provider 'virtualbox' do |vb|
          vb.memory = "2048"
          vb.name = "node-#{i}"
       end
       node.vm.provision 'shell', inline: <<-EOF
	yum install -y net-tools
	yum install -y java-devel
        wget "http://ftp.byfly.by/pub/apache.org/tomcat/tomcat-8/v8.5.23/bin/apache-tomcat-8.5.23.tar.gz"
        tar -xzvf "apache-tomcat-8.5.23.tar.gz"
        bash /home/vagrant/apache-tomcat-8.5.23/bin/startup.sh
	yum install -y avahi avahi-tools
	systemctl start avahi-daemon
	systemctl enable avahi-daemon
	wget "https://releases.hashicorp.com/serf/0.8.1/serf_0.8.1_linux_amd64.zip"
        sudo yum install -y unzip
        unzip serf_0.8.1_linux_amd64.zip 
        sudo mv serf /usr/bin/serf
        cp -f /vagrant/serf.service /etc/systemd/system/serf.service
        chown vagrant:vagrant /etc/systemd/system/serf.service
        chmod 777 /etc/systemd/system/serf.service
        systemctl daemon-reload
        systemctl enable serf.service
        systemctl start serf.service
       EOF
    end
  end

 config.vm.define "web" do |web| 
   web.vm.box = "sbeliakou/centos-7.4-x86_64-minimal"
   web.vm.hostname = "web"
   web.vm.network "private_network", ip: "172.16.1.70"
   web.ssh.insert_key = false
   web.vm.provider 'virtualbox' do |vb|
        vb.memory="1024"  
	vb.name = "web"
   end
   web.vm.provision 'shell', inline: <<-EOF
      yum install -y epel-release wget
      yum install -y net-tools	
      yum install -y nginx
      cp -f /vagrant/lb.conf /etc/nginx/conf.d/lb.conf
      systemctl start nginx.service
      systemctl enable nginx.service
      yum install -y avahi avahi-tools
      systemctl start avahi-daemon
      systemctl enable avahi-daemon
      wget "https://releases.hashicorp.com/serf/0.8.1/serf_0.8.1_linux_amd64.zip"
      sudo yum install -y unzip
      unzip serf_0.8.1_linux_amd64.zip 
      sudo mv serf /usr/bin/serf
      cp -f /vagrant/serf.service /etc/systemd/system/serf.service
      chown vagrant:vagrant /etc/systemd/system/serf.service
      chmod 777 /etc/systemd/system/serf.service
      systemctl daemon-reload
      systemctl enable serf.service
      systemctl start serf.service
  EOF
  end

  
end
