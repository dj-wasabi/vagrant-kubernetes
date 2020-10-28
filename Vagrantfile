IMAGE_NAME = "ubuntu/bionic64"
N = 2

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    config.vm.provider "virtualbox" do |v|
        v.memory = 2048
        v.cpus = 2
    end

    config.vm.define "control" do |control|
        control.vm.box = IMAGE_NAME
        control.vm.network "private_network", ip: "10.10.1.10"
        control.vm.hostname = "control"

        control.vm.provision "ansible" do |ansible|
            ansible.playbook = "control-playbook.yml"
            ansible.extra_vars = {
                node_ip: "10.10.1.10",
                k8s_version: "1.19.3",
                ansible_python_interpreter: "/usr/bin/python3",
            }
        end
    end

    (1..N).each do |i|
        config.vm.define "worker-#{i}" do |worker|
            worker.vm.box = IMAGE_NAME
            worker.vm.network "private_network", ip: "10.10.1.#{i + 10}"
            worker.vm.hostname = "worker-#{i}"
            worker.vm.provision "ansible" do |ansible|
                ansible.playbook = "worker-playbook.yml"
                ansible.extra_vars = {
                    control_node_ip: "10.10.1.10",
                    node_ip: "10.10.1.#{i + 10}",
                    k8s_version: "1.19.3",
                    ansible_python_interpreter: "/usr/bin/python3",
                }
            end
        end
    end
end