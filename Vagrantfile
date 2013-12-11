# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

$DATABASE_PROVISIONING_SCRIPT = <<SCRIPT

#Will be 1 if installed, or 0 if not
ANSIBLE_INSTALLED=`which ansible-playbook | wc -l`

if [ "$ANSIBLE_INSTALLED" == "1" ]; then

  echo "Skipping ansible install since it's already installed."

else
  echo "You are about to see some red error messages about tty and stdin, but can just ignore them."

  #first, update our apt-get cache
  sudo apt-get update

  #Now, install python-software-properties so we have the "add-apt-repository" command available
  sudo apt-get -y install python-software-properties
  sudo add-apt-repository ppa:rquillo/ansible

  #Now we run this again so that we get the very latest version of ansible's repository
  sudo apt-get update

  #finally, install ansible
  sudo apt-get -y install ansible
fi

echo "Provisioning with Ansible..."

# We set PYTHONUNBUFFERED to true so that ansible-playbook output appears "live" when Vagrant provisioning
sudo PYTHONUNBUFFERED=1 ansible-playbook /vagrant/provisioning/playbook.yml --connection=local -i "[database]127.0.0.1,"

SCRIPT


def is_windows?
  (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
end


def command_exists?(command)
  system("which #{ command} > /dev/null 2>&1")
end

def can_use_vagrant_provisioning?()
  !is_windows? && command_exists?('ansible')
end

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "precise64"

  # The url from where the 'config.vm.box' box will be fetched if it
  # doesn't already exist on the user's system.
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  # Filesystem configuration. Share the current folder as "/vagrant" on the guest VM
  config.vm.synced_folder "./", "/vagrant", :nfs=>true

  config.vm.define "database" do |cfg|
    cfg.vm.host_name = "dbserver.vm"
    # Forward port 5432 on the gust to 5432 on the host
    cfg.vm.network :forwarded_port, guest:5432, host:5432
    #The DB server will be at IP address 192.168.50.50
    cfg.vm.network :private_network, ip: "192.168.50.50"

    if can_use_vagrant_provisioning?
      #if we have ansible installed, let's use it from our host:
      puts "Ansible found on host. Using it for provisioning..."
      cfg.vm.provision :ansible do |ansible|
        ansible.playbook = "provisioning/playbook.yml"
        ansible.verbose = 'v' #Use more v's if you want more. Up to 3.
      end
    else
      puts "Ansible not found on host. Will use Ansible from with the guest VM..."
      config.vm.provision :shell,
        :inline => $DATABASE_PROVISIONING_SCRIPT
    end
  end
end
