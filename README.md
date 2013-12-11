

## Vagrant Ansible Postgres

This uses Ansible to provision a Vagrant box with Postgres 9.3.
If Ansible is not installed on the host machine, or if the host is a Windows machine, Ansible will be installed on the Vagrant guest box and run from there. This allows Windows devs to "vagrant up" just like the cool Mac kids.

## Requirements:

* Vagrant
* VirtualBox
* Ansible (optional)

## Usage

```
$ vagrant up
```

Your Vagrant box will now be running Postgresql, on localhost:5432.
The default user/password is: `pgsuper/password`
You can modify this and other playbook variables variables inside of `provisioning/group_vars/all`.

## License

I got a lot of the playbook ideas and code from [Kami/ansible-postgresql](https://github.com/Kami/ansible-postgresql), and as a result, this Playbook must be distributed under the
[Apache 2.0 license](http://www.apache.org/licenses/LICENSE-2.0.html).

