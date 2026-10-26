Headlamp is used as a frontend for the [[Kubernetes|/deploy/Kubernetes.md]] cluster. It covers all resources within the cluster and provides monitoring.

## Setup
On first install of Headlamp using the [[Ansible|/deploy/Ansible.md]] playbook, an additional step is required to complete installation.
After running the playbook, within the cluster, run:

`sudo kubectl create token headlamp -n kube-system`

to generate a token. Navigate to the [[headlamp URL|https://headlamp.jordanhome.dev]] and paste in the token. This will grant you access to the webpage.