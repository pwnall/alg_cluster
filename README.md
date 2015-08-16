## Requirements

The following [Homebrew](http://brew.sh) commands install the requirements on
OSX.

```bash
brew install ansible brew-cask python
brew cask install vagrant
vagrant plugin install vagrant-reload
```

## Inventory Errors

The dynamic inventory code was sourced from
[Ansible's repository](https://github.com/ansible/ansible/blob/devel/contrib/inventory/openstack.py).


## Running the Playbooks

First, copy `clouds.example.yaml` into `clouds.yaml` and insert the proper
credentials in it.

Second, run the cluster bringup playbook.

```bash
ansible-playbook -i "localhost," openstack_up.yml
```
