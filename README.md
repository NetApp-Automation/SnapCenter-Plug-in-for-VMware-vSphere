### Automated deployment of SnapCenter VMware Plugin using Ansible
 
This repository contains Ansible roles and playbooks for an automated deployment of SnapCenter VMware Plugin.

Note: Automated deployment of SnapCenter VMware Plugin requires minimum version of VMware vSphere 7.0 U2

The SnapCenter VMware Plugin deployment is based on the following role:

	snapcenter-vmware-plugin-config

### Environment Validated

The automation has been tested with the below versions of software:

	Storage Operating System: ONTAP 9.8 and 9.9
	VMware vSphere: 7.0.2 U2

### Prerequisites

1. It is assumed that a VMware vCenter instance has been setup and is available for use, similarly an ONTAP cluster that needs to be integrated with VMware vCenter will also need to be made available.

2. The user should have an Ansible Control machine that has network reachability to the ONTAP storage system, VMware vCenter and internet access to pull this repository from GitHub.
Refer https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html for guidance on setting up an Ansible Control machine.

3. The Ansible control machine should have the VMware dependency libraries and collections installed

```
pip3 install pyvmomi
ansible-galaxy collection install community.vmware
pip3 install --upgrade git+https://github.com/vmware/vsphere-automation-sdk-python.git
pip3 install -r ~/.ansible/collections/ansible_collections/community/vmware/requirements.txt
pip install aiohttp
ansible-galaxy collection install vmware.vmware_rest
```

### Getting Started

1. From the Ansible Control machine Download a ZIP version of this repository or clone it using the below command:
	
```
git clone https://github.com/NetApp-Automation/SnapCenter-Plug-in-for-VMware-vSphere.git
```

2. There is a variable file under the vars folder 'snapcenter_vmware_plugin_main.yml' for the setup of SnapCenter VMware Plugin, that need to be filled out with environment specific parameters prior to executing the playbook.

NOTE: The format of the variable file needs to be maintained as it is, any changes to the structure of the file may lead to failure in execution of the playbook.

NOTE: Sample values are pre-populated against some variables in order to provide the user additional clarity on how the variable needs to be filled out. Please replace the sample values with your environment specific information.

3. Update the credentials for VMware vCenter

Navigate to the 'vcenter' file within the 'group_vars' directory and update it with the admin credentials for VMware vCenter

Example -

	# This variable file is used by the playbooks for the Snapcenter VMware Plugin
	vcenter_username: "administrator@vsphere.local"
	vcenter_password: "password"

4. Update the Inventory file

Open the 'hosts' file and update it with a record for the vCenter IP

Example -

	[vcenter]
	# vCenter Management IP. List only one vCenter IP. This is used by the playbooks for Snapcenter VMware Plugin.
	192.168.3.150

5. Executing the Playbook

A playbook by name 'Setup_SnapCenter_VMware_Plugin.yml' is available at the root of this repository. It invokes the appropriate roles to complete the setup of Snapcenter VMware Plugin.

Execute the playbook from the Ansible Control machine as an admin/ root user using the following command:

	ansible-playbook -i hosts Setup_SnapCenter_VMware_Plugin.yml

### Authors

 * Jayakishore Esanakula (kishore.esanakula@netapp.com)

