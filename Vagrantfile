IMAGE_NAME = "ubuntu/bionic64"
N = 2

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    config.vm.provider "virtualbox" do |v|
        v.memory = 2048
        v.cpus = 2
    end

    config.vm.define "master" do |master|
        master.vm.box = IMAGE_NAME
        master.vm.network "private_network", ip: "10.10.1.10"
        master.vm.hostname = "master"

        master.vm.provision "ansible" do |ansible|
            ansible.playbook = "master-playbook.yml"
            ansible.extra_vars = {
                node_ip: "10.10.1.10",
                ansible_python_interpreter: "/usr/bin/python3",
            }
        end
    end

    (1..N).each do |i|
        config.vm.define "node-#{i}" do |node|
            node.vm.box = IMAGE_NAME
            node.vm.network "private_network", ip: "10.10.1.#{i + 10}"
            node.vm.hostname = "node-#{i}"
            node.vm.provision "ansible" do |ansible|
                ansible.playbook = "node-playbook.yml"
                ansible.extra_vars = {
                    node_ip: "10.10.1.#{i + 10}",
                    ansible_python_interpreter: "/usr/bin/python3",
                }
            end
        end
    end
end