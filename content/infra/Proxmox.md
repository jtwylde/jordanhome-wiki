Proxmox is the hypervisor of choice for the homelab. Proxmox is install bare-metal on the servers and is used to run the VMs in the homelab. Generally VMs are run using Debian.

In theory, since the homelab is built around [[Kubernetes|/infra/Kubernetes.md]], the control planes could be run bare-metal on the servers rather than on VMs. The choice was made to use VMs to still give flexibility for having VMs if needed, while leaving the K3S nodes to be entirely managed by [[IAC|/infra/Ansible.md]].

## Cluster
As of writing there are two proxmox nodes in the cluster. See [[Networking|/infra/Networking.md]] for networking details.

## Build
See the [[build page|/build/Proxmox Build Guide.md]] for a guide on building the nodes.
