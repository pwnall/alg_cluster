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

Due to
[an Ansible bug](https://github.com/ansible/ansible-modules-core/issues/1880),
a manual configuration step is necessary after bringing up the cluster.

1. In the OpenStack Horizon Web dashboard, go to _Networks_ under the
   _Network_ drop-down.
1. Under _Subnets_, locate the subnet created by the cluster bringup playbook
   (its name should resemble `alg-private-net`), and click _Edit Subnet_ in the
   _Actions_ dropdown.
1. Check the _Disable Gateway_ checkbox and click the _Next_ button, then click
   _Save_.


Third, run the Kubernetes bringup playbook.

```bash
ansible-playbook k8s.yml
```

Last, save the contents of the `keys/` directory somewhere safe.
