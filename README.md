

## Vagrant Ansible Postgres

This uses Ansible to provision a Vagrant box with Postgres 9.3. It optionally installs pgtap as well for use in testing your database configuration.
If Ansible is not installed on the host machine, or if the host is a Windows machine, Ansible will be installed on the Vagrant guest box and run from there. This allows Windows devs to "vagrant up" just like the cool Mac kids.

## Requirements:

* Vagrant
* VirtualBox
* Ansible (Optional: if not installed, will be installed in the Vagrant box and run locally)
* `nfsd` on the host, if host is Linux. (Hat tip to [davidemoro](https://github.com/Taytay/vagrant-ansible-postgres/issues/1))

## Usage

```
$ vagrant up
```


Your Vagrant box will now be running Postgresql, on localhost:5432.

The default user/password is: `pgsuper/password`

You can modify this and other playbook variables variables inside of `provisioning/group_vars/all`.

If you want to install [pgtap](http://pgtap.org/), which is helpful for unit testing your database, you can uncomment the pgtap role from the `playbook.yml` file.

Any time you make a change to the Ansible scripts and want to rerun provisioning:

```
$ vagrant provision
```


## License

I got a lot of the playbook ideas and code from [Kami/ansible-postgresql](https://github.com/Kami/ansible-postgresql), and as a result, this Playbook must be distributed under the
[Apache 2.0 license](http://www.apache.org/licenses/LICENSE-2.0.html).

