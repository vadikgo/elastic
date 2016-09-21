vm_list = ['els1', 'els2', 'els3']

Vagrant.configure(2) do |config|

  vm_list.each_with_index do |vm_name, vm_index|
      config.vm.box = "bento/centos-6.7"
      config.vm.define vm_name do |vmn|

          provider = (ARGV[2] || ENV['VAGRANT_DEFAULT_PROVIDER'] || :virtualbox).to_sym

          if provider == :virtualbox
              vmn.vm.provider "virtualbox" do |vb|
                vb.gui = false
                vb.memory = "2048"
              end
          end

          if provider == :parallels
              vmn.vm.provider "parallels" do |prl|
                prl.memory = "2048"
                prl.name = vm_name
                prl.linked_clone = true
                prl.update_guest_tools = false
                prl.check_guest_tools = false
                prl.customize ["set", :id, "--sh-app-guest-to-host", "off"]
                prl.customize ["set", :id, "--shared-cloud", "off"]
                prl.customize ["set", :id, "--shared-profile", "off"]
                prl.customize ["set", :id, "--time-sync", "on"]
              end
          end

        vmn.vm.hostname = vm_name
        vmn.vm.network :forwarded_port, guest: 9200, host: 9200+vm_index

        vmn.vm.provision "ansible" do |ansible|
          ansible.verbose = "v"
          ansible.groups = {
            "master" => vm_list[0],
            "data" => vm_list[1,2],
            "es_hosts:children" => ["master", "data"]
          }
          ansible.playbook = "multi_node.yml"
        end
    end
  end
end
