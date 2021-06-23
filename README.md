# vagrant-mip-deployment

This is the structure that you can use to deploy MIP development environments (originally on VirtualBox, but at the moment, it only works on OpenStack. I'll make it VirtualBox compatible again later).

## Required

* https://github.com/crochat/vagrant-installer
* https://github.com/HBPMedical/mip-deployment

## Folder structures

* dev_repos/mip_fed_to-be-imported_cde
  MIP CDEs folder. This content will be imported in MIP federated Master node. The import process will be managed by the "dev_repos/scripts/inside_vagrant_only.mip-import-cde" script.
* dev_repos/mip_fed_to-be-imported_data
  MIP datasets folder. This content will be imported in matching VMs (any MIP federated Worker node where VM name matches the subfolder name). The import process will be managed by the "dev_repos/scripts/inside_vagrant_only.mip-import-data" script.
* dev_repos/scripts
  Vagrant provisioning scripts folder. This contains the scripts which may be executed from within the VMs during the Vagrant's provisioning process.
* federated/fed1
  This is the first federated MIP Vagrant folder. You can clone it to create another federated MIP development environment.
* local/local1
  This is the first local MIP Vagrant folder. You can clone it to create another local MIP development environment.

## Pre-requirements preparation

* Install required tools

** rsshfs

```
git clone https://github.com/crochat/rsshfs
```

```
sudo install rsshfs/rsshfs /usr/local/bin
```

```
rm -rf rsshfs
```

** OpenStack

```
sudo apt-get update --fix-missing
```

```
sudo apt-get install -y --no-install-recommends git python3-nova python3-glance python3-swiftclient python3-keystone python3-neutron python3-cinder python3-ceilometer python3-heat lsb-core curl ruby gem gcc g++
```

** OpenStack configuration

You can git clone https://github.com/crochat/cscs-openstack-config and follow the README to prepare the configuration.
If you don't work on the CSCS OpenStack environment, you'll also have to adapt /etc/openstack/public-cloud.yaml configuration file.
To test it, you can run:

```
openstack --os-cloud YOUR_OPENSTACK_CLOUD_CONFIG_NAME flavor list
```

If it succeeds, you can continue.

If you cloned cscs-openstack-config, you can remove it

```
rm -rf cscs-openstack-config
```

** Vagrant

```
git clone https://github.com/crochat/vagrant-installer
```

**BEFORE YOU INSTALL VAGRANT**, if the latest version of Vagrant is 2.2.16, edit vagrant-installer/install_vagrant.sh and set VAGRANT_LATEST_RELEASE variable to 2.2.15!!
There's a bug in 2.2.16 which prevents Vagrant from succeeding in SSH authentication on guest machines!

```
sudo vagrant-installer/install_vagrant.sh
```

This install may **really** take a while, so be patient and don't interrupt it!

```
rm -rf vagrant-installer
```

## Vagrant deployment preparation

First, go in dev_repos folder and git clone the MIP development repo with

```
cd dev_repos
```

```
git clone https://github.com/HBPMedical/mip-deployment
```

This will be your MIP development folder, and it will be reachable from the various deployment configurations by mounted (bind type) dev_repos folder.
If, and **ONLY** if, you plan to work on the mip-deployment/mip script, you can also edit the script, search for variable MIP_COMMAND, and set it to "/vagrant/dev_repos/mip-deployment/mip".
Of course, **DO NOT, NEVER EVER, COMMIT THIS CHANGE**, as it must not be the default path for a "normal" MIP install!

If you want to deploy a local MIP, just go in local folder, then, or rename the local1 folder by the name of you machine, or go in it if you chose to name your VM local1.
Then, edit the vagrant-machines.yaml and replace all the <VARIABLES> which follow this pattern, by what fits your environment.
Once ready, you can mount the dev_repos folder here by running:

```
./mount-umount.sh
```

The dev_repos folder should appear.

After this step, if you followed everything and did all the required configurations well, you should be able to run:

```
vagrant status
```

It should not fail and return the status of your defined VM(s).
