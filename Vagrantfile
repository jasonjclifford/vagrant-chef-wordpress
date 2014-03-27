# -*- mode: ruby -*-
# vi: set ft=ruby :
$script = <<SCRIPT
echo I am provisioning...

echo 127.0.0.1 fonts.googleapis.com >> /etc/hosts
echo 127.0.0.1 1.gravatar.com >> /etc/hosts
echo 127.0.0.1 wordpress.org >> /etc/hosts
echo 127.0.0.1 gmpg.org >> /etc/hosts
echo 127.0.0.1 0.gravatar.com >> /etc/hosts
echo 127.0.0.1 http://www.w3.org >> /etc/hosts
echo 127.0.0.1 update.wordpress.org >> /etc/hosts
echo 127.0.0.1 api.wordpress.org >> /etc/hosts
echo 127.0.0.1 ajax.aspnetcdn.com >> /etc/hosts
echo 127.0.0.1 planet.wordpress.org >> /etc/hosts
echo 127.0.0.1 codex.wordpress.org >> /etc/hosts

SCRIPT


Vagrant.configure("2") do |config|
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  if Vagrant.has_plugin?("vagrant-cachier")
    # Configure cached packages to be shared between instances of the same base box.
    # More info on http://fgrehm.viewdocs.io/vagrant-cachier/usage
    config.cache.scope = :box

    # If you are using VirtualBox, you might want to use that to enable NFS for
    # shared folders. This is also very useful for vagrant-libvirt if you want
    # bi-directional sync
    # config.cache.synced_folder_opts = {
    #   type: :nfs,
    #   # The nolock option can be useful for an NFSv3 client that wants to avoid the
    #   # NLM sideband protocol. Without this option, apt-get might hang if it tries
    #   # to lock files needed for /var/cache/* operations. All of this can be avoided
    #   # by using NFSv4 everywhere. Please note that the tcp option is not the default.
    #   mount_options: ['rw', 'vers=3', 'tcp', 'nolock']
    # }
    # For more information please check http://docs.vagrantup.com/v2/synced-folders/basic_usage.html
  end

  # Uncomment the next line if you like to watch.
  config.vm.provider :virtualbox do |vb|
    # Don't boot with headless mode
    #vb.gui = true
  end

  # Port 8000 on the host will go to port 80 on the Vagrant box
  config.vm.network :forwarded_port, guest: 80, host: 8000, auto_correct: true

  # Here's a folder for passing stuff back and forth
  config.vm.synced_folder "./shared", "/home/vagrant/host_shared"

  config.vm.provision "shell", inline: $script

  config.vm.provision :chef_solo do |chef|
    chef.log_level = :debug
    chef.json = {
      :mysql => {
        :server_root_password   => "six pasta obsessed wreck",
        :server_repl_password   => "six pasta obsessed wreck",
        :server_debian_password => "six pasta obsessed wreck"
      }
    }
    chef.cookbooks_path = "cookbooks"
    #chef.add_recipe "apt"
    chef.add_recipe "ohai"
    chef.add_recipe "configure"
  end
end
