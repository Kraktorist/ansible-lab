$os = "bento/ubuntu-20.04"

Vagrant.configure("2") do |config|
    config.vm.define 'hmlabans01' do |config|
        config.vm.hostname = 'hmlabans01'
        config.vm.box = $os
        config.vm.network "public_network", ip: '192.168.0.41'
        config.vm.provision :shell, :inline =>"
            export DEBIAN_FRONTEND=noninteractive
            apt-get update
            apt-get -y install python3-pip
            pip3 install ansible
            mkdir /etc/ansible
            cat <<EOF >/etc/ansible/hosts
                all:
                children:
                appservers:
                    hosts:
                    192.168.0.42:
                    192.168.0.43:
                balancers:
                    hosts:
                    192.168.0.44:
            
            EOF
        ", privileged: true
        config.vm.provision "file", source: "~/.ssh/id_rsa", destination: "~/.ssh/id_rsa"
    end
end


Vagrant.configure("2") do |config|
    config.vm.define 'hmlabhost01' do |config|
        config.vm.hostname = 'hmlabhost01'
        config.vm.box = $os
        config.vm.network "public_network", ip: '192.168.0.42'
        ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
        config.vm.provision :shell, :inline =>"
            export DEBIAN_FRONTEND=noninteractive
            apt-get update
            echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
        ", privileged: true
    end
end

Vagrant.configure("2") do |config|
    config.vm.define 'hmlabhost02' do |config|
        config.vm.hostname = 'hmlabhost02'
        config.vm.box = $os
        config.vm.network "public_network", ip: '192.168.0.43'
        ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
        config.vm.provision :shell, :inline =>"
            export DEBIAN_FRONTEND=noninteractive
            apt-get update
            echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
        ", privileged: true
    end
end

Vagrant.configure("2") do |config|
    config.vm.define 'hmlabhost03' do |config|
        config.vm.hostname = 'hmlabhost03'
        config.vm.box = $os
        config.vm.network "public_network", ip: '192.168.0.44'
        ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
        config.vm.provision :shell, :inline =>"
            export DEBIAN_FRONTEND=noninteractive
            apt-get update
            echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
        ", privileged: true
    end
end