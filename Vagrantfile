IMAGE_NAME = "generic/ubuntu1804"
N = 2

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false
    config.vm.network "public_network"
    config.vm.synced_folder ".", "/vagrant", disabled: true
    config.vm.provider "hyperv" do |h|
        h.enable_virtualization_extensions = true
        h.linked_clone = true
    end

    config.vm.define "k8s-master" do |master|
        master.vm.box = IMAGE_NAME
        master.vm.hostname = "k8s-master"
        master.vm.provision "shell" do |sh|
            ssh_pub_key = File.readlines("./id_rsa.pub").first.strip
            sh.inline = <<-SHELL
                mkdir -p /home/vagrant/.ssh && chown -R vagrant /home/vagrant/.ssh
                echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
            SHELL
        end
    end

    (1..N).each do |i|
        config.vm.define "node-#{i}" do |node|
            node.vm.box = IMAGE_NAME
            node.vm.hostname = "node-#{i}"
            node.vm.provision "shell" do |sh|
                ssh_pub_key = File.readlines("./id_rsa.pub").first.strip
                sh.inline = <<-SHELL
                    mkdir -p /home/vagrant/.ssh && chown -R vagrant /home/vagrant/.ssh
                    echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
                SHELL
            end
        end
    end
end
