# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.

    # Docker EE node for CentOS 7.4
    config.vm.define "ucp" do |centos_ucp_node1|
      centos_ucp_node1.vm.box = "bento/centos-7.4"
      centos_ucp_node1.vm.network "private_network", ip: "172.28.128.31"
      centos_ucp_node1.landrush.tld = 'local'
      centos_ucp_node1.vm.hostname = "ucp.local"
      centos_ucp_node1.landrush.enabled = true
      centos_ucp_node1.landrush.host 'dtr.local', '172.28.128.34'
      centos_ucp_node1.landrush.host 'wordpress.local', '172.28.128.35'
      centos_ucp_node1.landrush.host 'jenkins.local', '172.28.128.35'
      centos_ucp_node1.landrush.host 'nodeapp.local', '172.28.128.35'
      centos_ucp_node1.landrush.host 'visualizer.local', '172.28.128.35'
      centos_ucp_node1.landrush.guest_redirect_dns = false
      centos_ucp_node1.landrush.upstream '8.8.8.8'
      centos_ucp_node1.landrush.upstream '8.8.4.4'
      config.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "2560"]
        vb.customize ["modifyvm", :id, "--cpus", "2"]
        vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
        vb.name = "centos-ucp-node1"
      end
      centos_ucp_node1.vm.provision "shell", inline: <<-SHELL
        sudo yum makecache fast
        sudo yum install -y ntpdate
        sudo ntpdate -s time.nist.gov
        sudo yum install -y epel-release
        sudo yum install -y jq
        sudo cp /vagrant/scripts/install_ee.sh .
        sudo cp /vagrant/scripts/install_ucp.sh .
        sudo cp /vagrant/scripts/configure_ucp.sh .
        sudo cp /vagrant/scripts/create_tokens.sh .
        sudo cp /vagrant/scripts/visualizer.sh .
        sudo cp /vagrant/scripts/backup_ucp.sh .
        sudo cp /vagrant/scripts/restore_ucp.sh .
        sudo chmod +x install_ee.sh
        sudo chmod +x install_ucp.sh
        sudo chmod +x configure_ucp.sh
        sudo chmod +x create_tokens.sh
        sudo chmod +x visualizer.sh
        sudo chmod +x backup_ucp.sh
        sudo chmod +x restore_ucp.sh
        ./install_ee.sh
        ./install_ucp.sh
        ./create_tokens.sh
        ./backup_ucp.sh
        # ./visualizer.sh
     SHELL
    end

    # Docker EE manager node for CentOS 7.4 (optional)
    config.vm.define "ucp-node2" do |centos_ucp_node2|
      centos_ucp_node2.vm.box = "bento/centos-7.4"
      centos_ucp_node2.vm.network "private_network", ip: "172.28.128.32"
      centos_ucp_node2.landrush.tld = 'local'
      centos_ucp_node2.vm.hostname = "ucp-node2.local"
      centos_ucp_node2.landrush.enabled = true
      centos_ucp_node2.landrush.upstream '8.8.8.8'
      centos_ucp_node2.landrush.upstream '8.8.4.4'
      centos_ucp_node2.landrush.guest_redirect_dns = false
      config.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "2500"]
        vb.customize ["modifyvm", :id, "--cpus", "2"]
        vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
        vb.name = "centos-ucp-node2"
      end
      centos_ucp_node2.vm.provision "shell", inline: <<-SHELL
        sudo yum install -y ntpdate jq
        sudo ntpdate -s time.nist.gov
        sudo cp /vagrant/scripts/install_ee.sh .
        sudo cp /vagrant/scripts/join_manager.sh .
        sudo chmod +x install_ee.sh
        sudo chmod +x join_manager.sh
        ./install_ee.sh
        ./join_manager.sh
      SHELL
    end

    # Docker EE manager node for CentOS 7.4 (optional)
    config.vm.define "ucp-node3" do |centos_ucp_node3|
      centos_ucp_node3.vm.box = "bento/centos-7.4"
      centos_ucp_node3.vm.network "private_network", ip: "172.28.128.33"
      centos_ucp_node3.landrush.tld = 'local'
      centos_ucp_node3.vm.hostname = "ucp-node3.local"
      centos_ucp_node3.landrush.enabled = true
      centos_ucp_node3.landrush.upstream '8.8.8.8'
      centos_ucp_node3.landrush.upstream '8.8.4.4'
      centos_ucp_node3.landrush.guest_redirect_dns = false
      config.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "2048"]
        vb.customize ["modifyvm", :id, "--cpus", "2"]
        vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
        vb.name = "ubuntu-ucp-node3"
      end
      centos_ucp_node3.vm.provision "shell", inline: <<-SHELL
        sudo yum install -y ntpdate
        sudo yum install -y epel-release
        sudo yum install -y jq
        sudo ntpdate -s time.nist.gov
        sudo cp /vagrant/scripts/install_ee.sh .
        sudo cp /vagrant/scripts/join_manager.sh .
        sudo chmod +x install_ee.sh
        sudo chmod +x join_manager.sh
        ./install_ee.sh
        ./join_manager.sh
     SHELL
    end

    # Docker EE DTR node for CentOS 7.4
    config.vm.define "dtr" do |centos_dtr_node1|
      centos_dtr_node1.vm.box = "bento/centos-7.4"
      centos_dtr_node1.vm.network "private_network", ip: "172.28.128.34"
      centos_dtr_node1.landrush.tld = 'local'
      centos_dtr_node1.vm.hostname = "dtr.local"
      centos_dtr_node1.landrush.enabled = true
      centos_dtr_node1.landrush.host 'ucp.local', '172.28.128.31'
      centos_dtr_node1.landrush.upstream '8.8.8.8'
      centos_dtr_node1.landrush.upstream '8.8.4.4'
      centos_dtr_node1.landrush.guest_redirect_dns = false
      config.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "2048"]
        vb.customize ["modifyvm", :id, "--cpus", "2"]
        vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
        vb.name = "ubuntu-dtr-node1"
      end
      centos_dtr_node1.vm.provision "shell", inline: <<-SHELL
        sudo yum install -y ntpdate
        sudo yum install -y epel-release
        sudo yum install -y jq
        sudo ntpdate -s time.nist.gov
        sudo cp /vagrant/scripts/install_ee.sh .
        sudo cp /vagrant/scripts/join_worker.sh .
        sudo cp /vagrant/scripts/install_dtr.sh .
        sudo cp /vagrant/scripts/prepopulate.sh .
        sudo cp /vagrant/scripts/backup_dtr.sh .
        sudo cp /vagrant/scripts/restore_dtr.sh .
        sudo chmod +x install_ee.sh
        sudo chmod +x join_worker.sh
        sudo chmod +x install_dtr.sh
        sudo chmod +x prepopulate.sh
        sudo chmod +x backup_dtr.sh
        sudo chmod +x restore_dtr.sh
        ./install_ee.sh
        ./join_worker.sh
        ./install_dtr.sh
        ./prepopulate.sh
        ./backup_dtr.sh
      SHELL
    end

    # Docker EE node for CentOS 7.4
    config.vm.define "worker-node1" do |centos_worker_node1|
      centos_worker_node1.vm.box = "bento/centos-7.4"
      centos_worker_node1.vm.network "private_network", ip: "172.28.128.35"
      centos_worker_node1.landrush.tld = 'local'
      centos_worker_node1.vm.hostname = "worker-node1.local"
      centos_worker_node1.landrush.enabled = true
      centos_worker_node1.landrush.upstream '8.8.8.8'
      centos_worker_node1.landrush.upstream '8.8.4.4'
      centos_worker_node1.landrush.guest_redirect_dns = false
      config.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "1024"]
        vb.customize ["modifyvm", :id, "--cpus", "2"]
        vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
        vb.name = "centos-worker-node1"
      end
      centos_worker_node1.vm.provision "shell", inline: <<-SHELL
        sudo yum install -y ntpdate
        sudo yum install -y epel-release
        sudo yum install -y jq
        sudo ntpdate -s time.nist.gov
        sudo cp /vagrant/scripts/install_ee.sh .
        sudo cp /vagrant/scripts/join_worker.sh .
        sudo chmod +x install_ee.sh
        sudo chmod +x join_worker.sh
        ./install_ee.sh
        ./join_worker.sh
     SHELL
    end

    # Docker EE node for CentOS 7.4
    config.vm.define "worker-node2" do |centos_worker_node2|
      centos_worker_node2.vm.box = "bento/centos-7.4"
      centos_worker_node2.vm.network "private_network", ip: "172.28.128.36"
      centos_worker_node2.landrush.tld = 'local'
      centos_worker_node2.vm.hostname = "worker-node2.local"
      centos_worker_node2.landrush.enabled = true
      centos_worker_node2.landrush.upstream '8.8.8.8'
      centos_worker_node2.landrush.upstream '8.8.4.4'
      centos_worker_node2.landrush.guest_redirect_dns = false
      config.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "1024"]
        vb.customize ["modifyvm", :id, "--cpus", "2"]
        vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
        vb.name = "centos-worker-node2"
      end
      centos_worker_node2.vm.provision "shell", inline: <<-SHELL
        sudo yum install -y ntpdate
        sudo yum install -y epel-release
        sudo yum install -y jq
        sudo ntpdate -s time.nist.gov
        sudo cp /vagrant/scripts/install_ee.sh .
        sudo cp /vagrant/scripts/join_worker.sh .
        sudo chmod +x install_ee.sh
        sudo chmod +x join_worker.sh
        sleep 5
        ./install_ee.sh
        ./join_worker.sh
     SHELL
    end

    # Gitlab node for CentOS 7.4 (https://docs.gitlab.com/omnibus/docker/)
    config.vm.define "gitlab" do |gitlab_node|
      gitlab_node.vm.box = "ubuntu/xenial64"
      gitlab_node.vm.network "private_network", ip: "172.28.128.37"
      gitlab_node.landrush.tld = 'local'
      gitlab_node.vm.hostname = "gitlab.local"
      gitlab_node.landrush.enabled = true
      config.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "2560"]
        vb.customize ["modifyvm", :id, "--cpus", "2"]
        vb.name = "gitlab-node"
      end
      gitlab_node.vm.provision "shell", inline: <<-SHELL
        sudo yum install -y ntpdate
        sudo yum install -y epel-release
        sudo yum install -y jq
        sudo ntpdate -s time.nist.gov
        sudo cp /vagrant/scripts/install_gitlab.sh .
        sudo cp /vagrant/scripts/configure_gitlab.sh .
        sudo chmod +x install_gitlab.sh
        sudo chmod +x configure_gitlab.sh
        sleep 5
        ./gitlab.sh
     SHELL
    end

    # Jenkins node for CentOS 7.4
    config.vm.define "jenkins" do |jenkins_node|
      jenkins_node.vm.box = "bento/centos-7.4"
      jenkins_node.vm.network "private_network", ip: "172.28.128.38"
      jenkins_node.landrush.tld = 'local'
      jenkins_node.vm.hostname = "jenkins-node.local"
      jenkins_node.landrush.enabled = true
      config.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "1024"]
        vb.customize ["modifyvm", :id, "--cpus", "2"]
        vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
        vb.name = "jenkins-node"
      end
      jenkins_node.vm.provision "shell", inline: <<-SHELL
        sudo yum install -y ntpdate
        sudo yum install -y epel-release
        sudo yum install -y jq unzip
        sudo ntpdate -s time.nist.gov
        sudo ntpdate -s time.nist.gov
        sudo cp /vagrant/scripts/install_ee.sh .
        sudo cp /vagrant/scripts/join_worker.sh .
        sudo cp /vagrant/scripts/deploy_jenkins.sh .
        sudo chmod +x install_ee.sh
        sudo chmod +x join_worker.sh
        sudo chmod +x deploy_jenkins.sh
        ./install_ee.sh
        ./join_worker.sh
        sleep 15
        ./deploy_jenkins.sh
     SHELL
    end

    # HAProxy for CentOS 7.4 (only utilized for HA)
    config.vm.define "haproxy" do |haproxy_node|
      haproxy_node.vm.box = "ubuntu/xenial64"
      haproxy_node.vm.network "private_network", ip: "172.28.128.30"
      haproxy_node.vm.hostname = "haproxy.local"
      config.vm.provider :virtualbox do |vb|
         vb.customize ["modifyvm", :id, "--memory", "512"]
         vb.customize ["modifyvm", :id, "--cpus", "1"]
         vb.name = "ubuntu-haproxy-node"
      end
      haproxy_node.landrush.enabled = true
      haproxy_node.landrush.tld = 'local'
      haproxy_node.landrush.host 'haproxy.local', '172.28.128.30'
      haproxy_node.landrush.host 'dtr.local', '172.28.128.30'
      haproxy_node.landrush.host 'ucp.local', '172.28.128.30'
      haproxy_node.landrush.host 'wordpress.local', '172.28.128.35'
      haproxy_node.landrush.host 'jenkins.local', '172.28.128.35'
      haproxy_node.landrush.host 'nodeapp.local', '172.28.128.35'
      haproxy_node.landrush.host 'visualizer.local', '172.28.128.35'
      haproxy_node.landrush.host 'gitlab.local', '172.28.128.31'
      haproxy_node.vm.provision "shell", inline: <<-SHELL
       sudo apt-get update
       sudo apt-get install -y apt-transport-https ca-certificates ntpdate
       sudo ntpdate -s time.nist.gov
       sudo apt-get install -y software-properties-common
       sudo add-apt-repository ppa:vbernat/haproxy-1.7
       sudo apt-get update
       sudo apt-get install -y haproxy
       ifconfig enp0s8 | grep 'inet addr:' | cut -d: -f2 | awk '{ print $1}' > /vagrant/haproxy-node
       sudo sed -i '/module(load="imudp")/s/^#//g' /etc/rsyslog.conf
       sudo sed -i '/input(type="imudp" port="514")/s/^#//g' /etc/rsyslog.conf
       sudo service rsyslog restart
       sudo cp /vagrant/files/haproxy.cfg /etc/haproxy/haproxy.cfg
       sudo service haproxy restart
      SHELL
    end
end
