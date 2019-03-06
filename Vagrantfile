API_VERSION = "2"
VAGRANT_BOX = "ubuntu/bionic64"

NO_MACHINES = 1

MACHINE_CPU = 2
MACHINE_MEMORY = 2048

ANSIBLE_GROUPS = {
    "machines" => ["machine[1:#{NO_MACHINES}]"],
    "all_groups:children" => ["machines"],
    "all_groups:vars" => {"ansible_python_interpreter" => "/usr/bin/python3"}
}

Vagrant.configure(API_VERSION) do |config|

    config.vm.box = VAGRANT_BOX

    (1..NO_MACHINES).each do |machine_id|
        config.vm.define "machine#{machine_id}" do |machine|
            machine.vm.define "machine#{machine_id}"
            machine.vm.network "private_network", ip: "10.3.2.#{1+machine_id}"
            machine.vm.provider "virtualbox" do |v|
                v.cpus = MACHINE_CPU
                v.memory = MACHINE_MEMORY
            end

            machine.vm.provision :ansible do |ansible|
                ansible.playbook = "ansible/prereqs.yml"
                ansible.groups = ANSIBLE_GROUPS
            end
        end
    end
end
