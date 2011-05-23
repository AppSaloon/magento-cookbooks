Vagrant::Config.run do |config|
  # Install base box from http://dl.dropbox.com/u/17995850/vagrant/natty32.box
  config.vm.box = "natty32"

  # Debug
  #config.vm.boot_mode = :gui
  
  config.vm.customize do |vm|
    vm.memory_size = 512
  end
 
  # Enable host-only networking, see http://vagrantup.com/docs/host_only_networking.html
  config.vm.network("33.33.33.33")

  config.vm.host_name = "magento.dev"

  # Enable provisioning with chef solo
  config.vm.provision :chef_solo do |chef|
    #chef.recipe_url = "http://cloud.github.com/downloads/tonigrigoriu/magento-cookbooks/cookbooks.tgz"
    chef.cookbooks_path = "~/workspace/cookbooks/magento_cookbooks"
    chef.add_recipe("vagrant-main")
    
    chef.json.merge!({
      :ubuntu => {
        :archive_url => "http://de.archive.ubuntu.com/ubuntu"
      },
      :mysql => {
        :server_root_password => "root"
      },      
      :apache => {
        :prefork => {
          :startservers => 5,
	  :minspareservers => 2,
          :maxspareservers => 2,
          :serverlimit => 10,
          :maxclients => 10,
          :maxrequestsperchild => 1000
        }
      },
      :magento => {
        :version => "1.5.1.0",
        :db => {
          :database => "magento",
          :user => "root",
          :password => "root"
        }
      },
      :nfs => {
        :exports => {
           "/var/www" => {
             :nfs_options => "33.33.33.1(rw,async,all_squash,anonuid=1000,anongid=1000,no_subtree_check,insecure)"
           }
        } 
      },
    })

    # Debug
    #chef.log_level = :debug
  end
end
