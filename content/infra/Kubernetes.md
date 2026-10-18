The homelab is built around a [[K3S|https://k3s.io]] cluster. This is a lightweight version of K8S suited for homelabs. 

## Structure 
As of writing there are two control planes in the cluster, which also serve as nodes. See [[Networking|/infra/ipam]] for networking details

Both control planes are ran from VMs on the Proxmox nodes.

## Contents
All objects in the cluster are managed by the [[Ansible|/infra/ansible]] playbooks. No changes should be made otherwise.

## Conventions
- Apps should be placed in their own namespaces (including all K8S objects [ingress, deployments, services, etc])
- Certs for HTTPS endpoints are issued by cert-manager. While testing, we should be using the `letsencrypt-staging` issuer. These are issued via ACME, with Cloudflare DNS validation