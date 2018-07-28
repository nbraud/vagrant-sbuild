Vagrant.configure("2") do |config|
  config.vm.box = "debian/contrib-stretch64"

  # Disable the new default behavior introduced in Vagrant 1.7, to
  # ensure that all Vagrant machines will use the same SSH key pair.
  # See https://github.com/mitchellh/vagrant/issues/5005
  config.ssh.insert_key = false

  config.vm.provider "virtualbox" do |vb|
#    vb.gui = true
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provision.yml"
  end

  config.vm.post_up_message = <<~HEREDOC
    Debian sbuild in-a-box, by nicoo
    https://github.com/nbraud/vagrant-sbuild
  HEREDOC
end
