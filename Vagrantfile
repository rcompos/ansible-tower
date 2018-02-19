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

  shared_folders_all = [
    { :host_dir => 'srv', :guest_dir => '/src', :create => 'false', :owner => 'vagrant', },
  ]
 
  ##########  Zero  ##########   

  config.vm.define 'zero' do |node|

    node.vm.box = "centos/7"
    node.vm.hostname = 'zero'
    node.vm.network "private_network", auto_network: true
    #node.vm.network "private_network", ip: '192.168.2.2'
    node.vm.provider :virtualbox do |vb|
      vb.gui = true
      vb.customize ['modifyvm', :id, '--memory', 2048]
      vb.customize ['modifyvm', :id, '--cpus', 2]
      vb.customize ['modifyvm', :id, '--vram', 128]
    end

    node.vm.provision :shell, inline: <<-SETUP
      if [[ ! -d /root/.ssh ]]; then mkdir -m0700 /root/.ssh; fi
      cp /vagrant/config/id_rsa* /root/.ssh
      if [[ -f /root/.ssh/id_rsa ]]; then chmod 0600 /root/.ssh/id_rsa; fi
      kfile='/root/.ssh/authorized_keys'; if [[ ! -z $kfile ]]; then cat /root/.ssh/id_rsa.pub > $kfile; fi && chmod 0600 $kfile
    SETUP

    node.vm.provision :shell, path:   "config/hostwithmost.pl"
    node.vm.provision :shell, inline: <<-SHELL 
      if rpm --quiet -q epel-release; then
        echo 'EPEL repo present'
      else
        echo 'Adding EPEL repo'
        /bin/rpm -ivh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
      fi
      #/bin/yum -y update
    SHELL

    ##if Vagrant.has_plugin? 'vagrant-hostmanager' ##  system "vagrant hostmanager" ##end
    node.vm.provision :shell, inline: "[[ ! -f /etc/yum.repos.d/epel-7.repo ]] || /bin/mv /etc/yum.repos.d/epel-7.repo /etc/yum.repos.d/epel-7.repo.disabled"
    node.vm.provision :shell, inline: 'perl -i -pe\'s/^SELINUX=enforcing\s+$/SELINUX=disabled\n/\' /etc/selinux/config'
    node.vm.provision :shell, inline: "yum -y install git bash-completion tree bind-utils elinks lynx fortune-mod cowsay python2-pip wget net-tools ansible mariadb"
    node.vm.provision :shell, inline: "/bin/pip install --disable-pip-version-check -q cryptography"
    node.vm.provision :shell, inline: "[[ -f /root/.gitconfig ]] || touch /root/.gitconfig"
    node.vm.provision :shell, path:   "config/gitconfiger.pl"
    node.vm.provision :shell, inline: "[[ -f /root/.bashrc ]] || touch /root/.bashrc"
    node.vm.provision :shell, path:   "config/bashrc_mod.pl"
    node.vm.provision :shell, path:   "config/fortune_cowsy.sh"
    node.vm.provision :shell, inline: "yum group install -y 'GNOME Desktop' 'Graphical Administration Tools'"
    node.vm.provision :shell, inline: "systemctl set-default graphical.target"

    #node.vm.synced_folder "srv", "/srv", create: true, owner: 'vagrant'
    shared_folders_all.each do |shared|
      hostdir  = shared[:host_dir]
      guestdir = shared[:guest_dir]
      create   = shared[:create]
      owner    = shared[:owner]
      node.vm.synced_folder "../#{hostdir}", "/#{guestdir}", create: "#{create}, owner: "#{owner}"
    end

  end

  ##########  kube0  ##########   

  config.vm.define 'kube0' do |node|

    node.vm.box = "centos/7"
    node.vm.hostname = 'kube0'
    node.vm.network "private_network", auto_network: true
    #node.vm.network "private_network", ip: '192.168.2.2'
    node.vm.provider :virtualbox do |vb|
      vb.gui = true
      vb.customize ['modifyvm', :id, '--memory', 2048]
      vb.customize ['modifyvm', :id, '--cpus', 2]
      vb.customize ['modifyvm', :id, '--vram', 128]
    end

    node.vm.provision :shell, inline: <<-SETUP
      if [[ ! -d /root/.ssh ]]; then mkdir -m0700 /root/.ssh; fi
      cp /vagrant/config/id_rsa* /root/.ssh
      if [[ -f /root/.ssh/id_rsa ]]; then chmod 0600 /root/.ssh/id_rsa; fi
      kfile='/root/.ssh/authorized_keys'; if [[ ! -z $kfile ]]; then cat /root/.ssh/id_rsa.pub > $kfile; fi && chmod 0600 $kfile
    SETUP

    node.vm.provision :shell, path:   "config/hostwithmost.pl"
    node.vm.provision :shell, inline: <<-SHELL 
      if rpm --quiet -q epel-release; then
        echo 'EPEL repo present'
      else
        echo 'Adding EPEL repo'
        /bin/rpm -ivh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
      fi
      #/bin/yum -y update
    SHELL

    ##if Vagrant.has_plugin? 'vagrant-hostmanager' ##  system "vagrant hostmanager" ##end
    node.vm.provision :shell, inline: "[[ ! -f /etc/yum.repos.d/epel-7.repo ]] || /bin/mv /etc/yum.repos.d/epel-7.repo /etc/yum.repos.d/epel-7.repo.disabled"
    node.vm.provision :shell, inline: 'perl -i -pe\'s/^SELINUX=enforcing\s+$/SELINUX=disabled\n/\' /etc/selinux/config'
    node.vm.provision :shell, inline: "yum -y install git bash-completion tree bind-utils elinks lynx fortune-mod cowsay python2-pip wget net-tools ansible"
    node.vm.provision :shell, inline: "/bin/pip install --disable-pip-version-check -q cryptography"
    node.vm.provision :shell, inline: "[[ -f /root/.gitconfig ]] || touch /root/.gitconfig"
    node.vm.provision :shell, path:   "config/gitconfiger.pl"
    node.vm.provision :shell, inline: "[[ -f /root/.bashrc ]] || touch /root/.bashrc"
    node.vm.provision :shell, path:   "config/bashrc_mod.pl"
    node.vm.provision :shell, path:   "config/fortune_cowsy.sh"
    node.vm.provision :shell, inline: "yum group install -y 'GNOME Desktop' 'Graphical Administration Tools'"
    node.vm.provision :shell, inline: "systemctl set-default graphical.target"

    #node.vm.synced_folder "srv", "/srv", create: true, owner: 'vagrant'
    shared_folders_all.each do |shared|
      hostdir  = shared[:host_dir]
      guestdir = shared[:guest_dir]
      create   = shared[:create]
      owner    = shared[:owner]
      node.vm.synced_folder "../#{hostdir}", "/#{guestdir}", create: "#{create}, owner: "#{owner}"
    end

  end

  ##########  xkube0  ##########   

  config.vm.define 'xkube0' do |node|

    node.vm.box = "centos/7"
    node.vm.hostname = 'xkube0'
    node.vm.network "private_network", auto_network: true
    #node.vm.network "private_network", ip: '192.168.2.2'
    node.vm.provider :virtualbox do |vb|
      vb.gui = true
      vb.customize ['modifyvm', :id, '--memory', 4096]
      vb.customize ['modifyvm', :id, '--cpus', 2]
      vb.customize ['modifyvm', :id, '--vram', 128]
    end

    node.vm.provision :shell, inline: <<-SETUP
      if [[ ! -d /root/.ssh ]]; then mkdir -m0700 /root/.ssh; fi
      cp /vagrant/config/id_rsa* /root/.ssh
      if [[ -f /root/.ssh/id_rsa ]]; then chmod 0600 /root/.ssh/id_rsa; fi
      kfile='/root/.ssh/authorized_keys'; if [[ ! -z $kfile ]]; then cat /root/.ssh/id_rsa.pub > $kfile; fi && chmod 0600 $kfile
    SETUP

    node.vm.provision :shell, path:   "config/hostwithmost.pl"
    node.vm.provision :shell, inline: <<-SHELL 
      if rpm --quiet -q epel-release; then
        echo 'EPEL repo present'
      else
        echo 'Adding EPEL repo'
        /bin/rpm -ivh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
      fi
      #/bin/yum -y update
    SHELL

    ##if Vagrant.has_plugin? 'vagrant-hostmanager' ##  system "vagrant hostmanager" ##end
    node.vm.provision :shell, inline: "[[ ! -f /etc/yum.repos.d/epel-7.repo ]] || /bin/mv /etc/yum.repos.d/epel-7.repo /etc/yum.repos.d/epel-7.repo.disabled"
    node.vm.provision :shell, inline: 'perl -i -pe\'s/^SELINUX=enforcing\s+$/SELINUX=disabled\n/\' /etc/selinux/config'
    node.vm.provision :shell, inline: "yum -y install git bash-completion tree bind-utils elinks lynx fortune-mod cowsay python2-pip wget net-tools ansible"
    node.vm.provision :shell, inline: "/bin/pip install --disable-pip-version-check -q cryptography"
    node.vm.provision :shell, inline: "[[ -f /root/.gitconfig ]] || touch /root/.gitconfig"
    node.vm.provision :shell, path:   "config/gitconfiger.pl"
    node.vm.provision :shell, inline: "[[ -f /root/.bashrc ]] || touch /root/.bashrc"
    node.vm.provision :shell, path:   "config/bashrc_mod.pl"
    node.vm.provision :shell, path:   "config/fortune_cowsy.sh"
    node.vm.provision :shell, inline: "yum group install -y 'GNOME Desktop' 'Graphical Administration Tools'"
    node.vm.provision :shell, inline: "systemctl set-default graphical.target"

    #node.vm.synced_folder "srv", "/srv", create: true, owner: 'vagrant'
    shared_folders_all.each do |shared|
      hostdir  = shared[:host_dir]
      guestdir = shared[:guest_dir]
      create   = shared[:create]
      owner    = shared[:owner]
      node.vm.synced_folder "../#{hostdir}", "/#{guestdir}", create: "#{create}, owner: "#{owner}"
    end

  end

  ##########  xkube1  ##########   

  config.vm.define 'xkube1' do |node|

    node.vm.box = "centos/7"
    node.vm.hostname = 'xkube1'
    node.vm.network "private_network", auto_network: true
    #node.vm.network "private_network", ip: '192.168.2.2'
    node.vm.provider :virtualbox do |vb|
      #vb.gui = true
      vb.customize ['modifyvm', :id, '--memory', 2048]
      vb.customize ['modifyvm', :id, '--cpus', 2]
      vb.customize ['modifyvm', :id, '--vram', 128]
    end

    node.vm.provision :shell, inline: <<-SETUP
      if [[ ! -d /root/.ssh ]]; then mkdir -m0700 /root/.ssh; fi
      cp /vagrant/config/id_rsa* /root/.ssh
      if [[ -f /root/.ssh/id_rsa ]]; then chmod 0600 /root/.ssh/id_rsa; fi
      kfile='/root/.ssh/authorized_keys'; if [[ ! -z $kfile ]]; then cat /root/.ssh/id_rsa.pub > $kfile; fi && chmod 0600 $kfile
    SETUP

    node.vm.provision :shell, path:   "config/hostwithmost.pl"
    node.vm.provision :shell, inline: <<-SHELL 
      if rpm --quiet -q epel-release; then
        echo 'EPEL repo present'
      else
        echo 'Adding EPEL repo'
        /bin/rpm -ivh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
      fi
      #/bin/yum -y update
    SHELL

    ##if Vagrant.has_plugin? 'vagrant-hostmanager' ##  system "vagrant hostmanager" ##end
    node.vm.provision :shell, inline: "[[ ! -f /etc/yum.repos.d/epel-7.repo ]] || /bin/mv /etc/yum.repos.d/epel-7.repo /etc/yum.repos.d/epel-7.repo.disabled"
    node.vm.provision :shell, inline: 'perl -i -pe\'s/^SELINUX=enforcing\s+$/SELINUX=disabled\n/\' /etc/selinux/config'
    node.vm.provision :shell, inline: "yum -y install git bash-completion tree bind-utils elinks lynx fortune-mod cowsay python2-pip wget net-tools ansible"
    node.vm.provision :shell, inline: "/bin/pip install --disable-pip-version-check -q cryptography"
    node.vm.provision :shell, inline: "[[ -f /root/.gitconfig ]] || touch /root/.gitconfig"
    node.vm.provision :shell, path:   "config/gitconfiger.pl"
    node.vm.provision :shell, inline: "[[ -f /root/.bashrc ]] || touch /root/.bashrc"
    node.vm.provision :shell, path:   "config/bashrc_mod.pl"
    node.vm.provision :shell, path:   "config/fortune_cowsy.sh"
    node.vm.provision :shell, inline: "yum group install -y 'GNOME Desktop' 'Graphical Administration Tools'"
    node.vm.provision :shell, inline: "systemctl set-default graphical.target"

    #node.vm.synced_folder "srv", "/srv", create: true, owner: 'vagrant'
    shared_folders_all.each do |shared|
      hostdir  = shared[:host_dir]
      guestdir = shared[:guest_dir]
      create   = shared[:create]
      owner    = shared[:owner]
      node.vm.synced_folder "../#{hostdir}", "/#{guestdir}", create: "#{create}, owner: "#{owner}"
    end

  end

  ##########  xkube2  ##########   

  config.vm.define 'core01' do |node|

    node.vm.box = "coreos-alpha"
    node.vm.box_url = "https://alpha.release.core-os.net/amd64-usr/current/coreos_production_vagrant_virtualbox.json"
    node.vm.hostname = 'core01'
    node.vm.network "private_network", auto_network: true
    node.vm.provider :virtualbox do |vb|
      #vb.gui = true
      vb.customize ['modifyvm', :id, '--memory', 2048]
      vb.customize ['modifyvm', :id, '--cpus', 2]
      vb.customize ['modifyvm', :id, '--vram', 64]
    end

    node.vm.provision :shell, inline: <<-SETUP
      if [[ ! -d /root/.ssh ]]; then mkdir -m0700 /root/.ssh; fi
      cp /vagrant/config/id_rsa* /root/.ssh
      if [[ -f /root/.ssh/id_rsa ]]; then chmod 0600 /root/.ssh/id_rsa; fi
      kfile='/root/.ssh/authorized_keys'; if [[ ! -z $kfile ]]; then cat /root/.ssh/id_rsa.pub > $kfile; fi && chmod 0600 $kfile
    SETUP

    shared_folders_all.each do |shared|
      hostdir  = shared[:host_dir]
      guestdir = shared[:guest_dir]
      create   = shared[:create]
      owner    = shared[:owner]
      node.vm.synced_folder "../#{hostdir}", "/#{guestdir}", create: "#{create}, owner: "#{owner}"
    end

  end

  ##########  k8s0  ##########   

  config.vm.define 'k8s0' do |node|

    node.vm.box = "ubuntu/xenial64"
    node.vm.hostname = 'k8s0'
    node.vm.network "private_network", auto_network: true
    #node.vm.network "private_network", ip: '192.168.2.2'
    node.vm.provider :virtualbox do |vb|
      #vb.gui = true
      vb.customize ['modifyvm', :id, '--memory', 2048]
      vb.customize ['modifyvm', :id, '--cpus', 2]
      vb.customize ['modifyvm', :id, '--vram', 64]
    end

    node.vm.provision :shell, inline: <<-SETUP
      if [[ ! -d /root/.ssh ]]; then mkdir -m0700 /root/.ssh; fi
      cp /vagrant/config/id_rsa* /root/.ssh
      if [[ -f /root/.ssh/id_rsa ]]; then chmod 0600 /root/.ssh/id_rsa; fi
      kfile='/root/.ssh/authorized_keys'; if [[ ! -z $kfile ]]; then cat /root/.ssh/id_rsa.pub > $kfile; fi && chmod 0600 $kfile
    SETUP

    #node.vm.provision :shell, path:   "config/hostwithmost-ubu.pl"

    ##if Vagrant.has_plugin? 'vagrant-hostmanager' ##  system "vagrant hostmanager" ##end

    #node.vm.synced_folder "srv", "/srv", create: true, owner: 'vagrant'
    shared_folders_all.each do |shared|
      hostdir  = shared[:host_dir]
      guestdir = shared[:guest_dir]
      create   = shared[:create]
      owner    = shared[:owner]
      node.vm.synced_folder "../#{hostdir}", "/#{guestdir}", create: "#{create}, owner: "#{owner}"
    end

  end

  ##########  k8s1  ##########

  config.vm.define 'k8s1' do |node|

    node.vm.box = "ubuntu/xenial64"
    node.vm.hostname = 'k8s1'
    node.vm.network "private_network", auto_network: true
    #node.vm.network "private_network", ip: '192.168.2.2'
    node.vm.provider :virtualbox do |vb|
      #vb.gui = true
      vb.customize ['modifyvm', :id, '--memory', 2048]
      vb.customize ['modifyvm', :id, '--cpus', 2]
      vb.customize ['modifyvm', :id, '--vram', 64]
    end

    node.vm.provision :shell, inline: <<-SETUP
      if [[ ! -d /root/.ssh ]]; then mkdir -m0700 /root/.ssh; fi
      cp /vagrant/config/id_rsa* /root/.ssh
      if [[ -f /root/.ssh/id_rsa ]]; then chmod 0600 /root/.ssh/id_rsa; fi
      kfile='/root/.ssh/authorized_keys'; if [[ ! -z $kfile ]]; then cat /root/.ssh/id_rsa.pub > $kfile; fi && chmod 0600 $kfile
    SETUP

    #node.vm.provision :shell, path:   "config/hostwithmost.pl"

    ##if Vagrant.has_plugin? 'vagrant-hostmanager' ##  system "vagrant hostmanager" ##end

    #node.vm.synced_folder "srv", "/srv", create: true, owner: 'vagrant'
    shared_folders_all.each do |shared|
      hostdir  = shared[:host_dir]
      guestdir = shared[:guest_dir]
      create   = shared[:create]
      owner    = shared[:owner]
      node.vm.synced_folder "../#{hostdir}", "/#{guestdir}", create: "#{create}, owner: "#{owner}"
    end

  end

  ##########  k8s2  ##########

  config.vm.define 'k8s2' do |node|

    node.vm.box = "ubuntu/xenial64"
    node.vm.hostname = 'k8s2'
    node.vm.network "private_network", auto_network: true
    #node.vm.network "private_network", ip: '192.168.2.2'
    node.vm.provider :virtualbox do |vb|
      #vb.gui = true
      vb.customize ['modifyvm', :id, '--memory', 2048]
      vb.customize ['modifyvm', :id, '--cpus', 2]
      vb.customize ['modifyvm', :id, '--vram', 64]
    end

    node.vm.provision :shell, inline: <<-SETUP
      if [[ ! -d /root/.ssh ]]; then mkdir -m0700 /root/.ssh; fi
      cp /vagrant/config/id_rsa* /root/.ssh
      if [[ -f /root/.ssh/id_rsa ]]; then chmod 0600 /root/.ssh/id_rsa; fi
      kfile='/root/.ssh/authorized_keys'; if [[ ! -z $kfile ]]; then cat /root/.ssh/id_rsa.pub > $kfile; fi && chmod 0600 $kfile
    SETUP

    #node.vm.provision :shell, path:   "config/hostwithmost.pl"

    ##if Vagrant.has_plugin? 'vagrant-hostmanager' ##  system "vagrant hostmanager" ##end

    #node.vm.synced_folder "srv", "/srv", create: true, owner: 'vagrant'
    shared_folders_all.each do |shared|
      hostdir  = shared[:host_dir]
      guestdir = shared[:guest_dir]
      create   = shared[:create]
      owner    = shared[:owner]
      node.vm.synced_folder "../#{hostdir}", "/#{guestdir}", create: "#{create}, owner: "#{owner}"
    end

  end

  ##########  CentOS 7 VM's  ##########   

  centos7vms = [
    { :hostname => 'cent7s0', :box      => 'centos/7', :cpu      => 1, :ram      => 1024, },
    { :hostname => 'cent7s1', :box      => 'centos/7', :cpu      => 2, :ram      => 2048, },
    { :hostname => 'cent7s2', :box      => 'centos/7', :cpu      => 2, :ram      => 4096, },
    { :hostname => 'cicd0',   :box      => 'centos/7', :cpu      => 2, :ram      => 4096, },
    { :hostname => 'cicd1',   :box      => 'centos/7', :cpu      => 2, :ram      => 4096, },
    { :hostname => 'cicd2',   :box      => 'centos/7', :cpu      => 2, :ram      => 4096, },
  ]

  centos7vms.each do |machine|
    config.vm.define machine[:hostname] do |cent7|

      cent7.vm.box = machine[:box]
      cent7.vm.hostname = machine[:hostname]
      #cent7.vm.network :'private_network', ip: '192.168.33.10'
      #cent7.vm.network 'forwarded_port', guest: 80, host: 8880, :auto_correct => true
      #cent7.hostmanager.enabled = true
      #cent7.hostmanager.manage_host = true
      #cent7.vm.network 'private_network', ip: machine[:ip]
      cent7.vm.network "private_network", auto_network: true
      cent7.vm.provider 'virtualbox' do |vb|
        vb.customize ['modifyvm', :id, '--memory', machine[:ram]]
        vb.customize ['modifyvm', :id, '--cpus', machine[:cpu]]
      end
 
      cent7.vm.provision :shell, inline: <<-SETUP
        if [[ ! -d /root/.ssh ]]; then mkdir -m0700 /root/.ssh; fi
        cp /vagrant/config/id_rsa* /root/.ssh
        if [[ -f /root/.ssh/id_rsa ]]; then chmod 0600 /root/.ssh/id_rsa; fi
        kfile='/root/.ssh/authorized_keys'; if [[ ! -z $kfile ]]; then cat /root/.ssh/id_rsa.pub > $kfile; fi && chmod 0600 $kfile
      SETUP

      shared_folders_all.each do |shared|
        hostdir  = shared[:host_dir]
        guestdir = shared[:guest_dir]
        create   = shared[:create]
        owner    = shared[:owner]
        cent7.vm.synced_folder "../#{hostdir}", "/#{guestdir}", create: "#{create}, owner: "#{owner}"
      end

    end
  end

  ##########  CentOS 6 VM's  ##########   

  centos6vms = [
    { :hostname => 'cent6s0', :box      => 'centos/6', :cpu      => 1, :ram      => 1024, },
    #{ :hostname => 'cent6s1', :box      => 'centos/6', :cpu      => 1, :ram      => 1024, },
  ]

  centos6vms.each do |machine|
    config.vm.define machine[:hostname] do |cent6|

      cent6.vm.box = machine[:box]
      cent6.vm.hostname = machine[:hostname]
      cent6.vm.provider 'virtualbox' do |vb|
        vb.customize ['modifyvm', :id, '--memory', machine[:ram]]
      end
      cent6.vm.network :private_network, :auto_network => true

      cent6.vm.provision :shell, inline: <<-SETUP
        if [[ ! -d /root/.ssh ]]; then mkdir -m0700 /root/.ssh; fi
        cp /vagrant/config/id_rsa* /root/.ssh
        if [[ -f /root/.ssh/id_rsa ]]; then chmod 0600 /root/.ssh/id_rsa; fi
        kfile='/root/.ssh/authorized_keys'; if [[ ! -z $kfile ]]; then cat /root/.ssh/id_rsa.pub > $kfile; fi && chmod 0600 $kfile
        restorecon -Rv ~/.ssh
      SETUP

      cent6.vm.provision :shell, inline: <<-SHELL 
        if rpm --quiet -q epel-release; then
          echo 'EPEL repo present'
        else
          echo 'Adding EPEL repo'
          rpm -ivh https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm
        fi
        #yum -y update
      SHELL

      cent6.vm.provision :shell, inline: 'perl -i -pe\'s/^SELINUX=enforcing\s+$/SELINUX=disabled\n/\' /etc/selinux/config'
      #cent6.vm.provision :shell, inline: "[[ -f /etc/profile.d/motd.sh ]] || echo '/bin/fortune | /bin/cowsay' > /etc/profile.d/motd.sh"
      cent6.vm.provision :shell, inline: "yum -y install fortune-mod cowsay"
      cent6.vm.provision :shell, path:   "config/fortune_cowsy.sh"

      shared_folders_all.each do |shared|
        hostdir  = shared[:host_dir]
        guestdir = shared[:guest_dir]
        create   = shared[:create]
        owner    = shared[:owner]
        cent6.vm.synced_folder "../#{hostdir}", "/#{guestdir}", create: "#{create}, owner: "#{owner}"
      end

    end
  end

  ##########  Ubuntu 14 VM's  ##########   

  ubuntu14vms = [
    #{ :hostname => 'ubu14s0', :box      => 'ubuntu/trusty64', :cpu      => 1, :ram      => 512, },
    #{ :hostname => 'ubu14s1', :box      => 'ubuntu/trusty64', :cpu      => 1, :ram      => 512, },
  ]

  ubuntu14vms.each do |machine|
    config.vm.define machine[:hostname] do |ubu|
      ubu.vm.box      = machine[:box]
      ubu.vm.hostname = machine[:hostname]
      ubu.vm.provider 'virtualbox' do |vb|
        vb.customize ['modifyvm', :id, '--memory', machine[:ram]]
      end
      ubu.vm.network :private_network, :auto_network => true
      ubu.vm.provision :shell, inline: "if [[ ! -d /root/.ssh ]]; then mkdir -m0700 /root/.ssh; fi"
      ubu.vm.provision :shell, inline: "cp /vagrant/config/id_rsa* /root/.ssh"
      ubu.vm.provision :shell, inline: "kfile='/root/.ssh/authorized_keys'; if [[ ! -z $kfile ]]; then cat /root/.ssh/id_rsa.pub > $kfile; fi && chmod 0600 $kfile"
    end   
  end   

end 
