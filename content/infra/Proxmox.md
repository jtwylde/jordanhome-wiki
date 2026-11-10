Proxmox is the hypervisor of choice for the homelab. Proxmox is install bare-metal on the servers and is used to run the VMs in the homelab. Generally VMs are run using Debian.

In theory, since the homelab is built around [[Kubernetes|/deploy/Kubernetes.md]], the control planes could be run bare-metal on the servers rather than on VMs. The choice was made to use VMs to still give flexibility for having VMs if needed, while leaving the K3S nodes to be entirely managed by [[IAC|/deploy/Ansible.md]].

## Cluster
As of writing there are two proxmox nodes in the cluster. See [[Networking|/infra/Networking.md]] for networking details.

## Cloud-init & Templates

### Standard Image Template
We use cloud-init to build out VMs, it is based on a standard Debian 13 image. The template is currently duplicated onto each node. The reason for this is that the template acts as the backend storage for each VM that clones it. We use LVM-thin local storage on each of the nodes, meaning that templates must exist locally in order to clone from them.

### Cloud-init
Cloud-init is used to initialize default settings for the VMs. There are a couple of configs we take advantage of here alongside [[Ansible|/deploy/Ansible.md]].

There are various configs we use below, and they are designed to work around Proxmox weirdness with cloud-init.

#### User-data Config
The user config is automatically generated from Proxmox. It will pass through the default user (root), default password for that user, hostname, and whether or not to automatically update packages on boot. It is configured through the Proxmox GUI only, on the templates themselves. Having Proxmox generate it is the only way to get the hostname passed through.

#### Vendor Config
Vendor configs are merged with user configs on creation. We set a custom vendor config on the template through the CLI, and the config itself is stored on the [[NAS|/infra/NAS.md]]. We specify the default users (my admin user and the [[ansible|/deploy/Ansible.md]] user), as well as SSH keys for those users in here.

The CLI command to set the custom CI vendor config:

`qm set <template_vmid> --cicustom vendor=NAS:snippets/base_debian.yaml`

Cloud-init merges vendor configs on top of user-data, but user-data will always take precedence, and it will not append/replace keys. 

**In other words, if a key is already set in user-data, it will ignore that same key in vendor data.**

By default, Proxmox will create a `users` list in user-data to define default users. This is a problem when trying to pass in custom users through vendor-data like we do. To get around this, we take advantage of a quirk in Proxmox where if the default user is set to **root** then it will not create the `users` list in user-data. See [[here|https://forum.proxmox.com/threads/cloud-init-brain-dump.170288/]].
> This is not well-defined behaviour and could be prone to breaking


#### Configs from Ansible
Finally, we want to set some settings from [[Ansible|/deploy/Ansible.md]], namely network settings. We use the inventory in [[Ansible|/deploy/Ansible.md]] to pass through a VMs static IP & gateway. Importantly, in the playbook this is done in a seperate step from cloning the VM due to Proxmox weirdness.

## Templates
A standard Debian template is used for cloning all VMs in the environment. It is currently duplicated onto each node. 

## Build
See the [[build page|/build/Proxmox Build Guide.md]] for a guide on building the nodes.
