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
