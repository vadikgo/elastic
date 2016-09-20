Vagrant.configure(2) do |config|

  config.vm.box = "bento/centos-6.7"

  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = "2048"
    vb.gui = false
    vb.linked_clone = true
  end

  config.vm.provider "parallels" do |prl|
    prl.memory = "2048"
    prl.linked_clone = true
    prl.update_guest_tools = false
    prl.check_guest_tools = false
    prl.customize ["set", :id, "--sh-app-guest-to-host", "off"]
    prl.customize ["set", :id, "--shared-cloud", "off"]
    prl.customize ["set", :id, "--shared-profile", "off"]
    prl.customize ["set", :id, "--time-sync", "on"]
  end

  config.vm.hostname = "es1"
  config.vm.define "es1"

  config.vm.provision "ansible" do |ansible|
    ansible.verbose = "v"
    ansible.groups = {
      "es" => ["es1"]
    }
    ansible.playbook = "install.yml"
  end
end
