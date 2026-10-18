
This page details the steps taken to build the proxmox nodes used in jordanhome.

Read through each section and follow in order.

## Flash ISO
Download the [Proxmxo ISO](https://www.proxmox.com/en/downloads/proxmox-virtual-environment/iso)

Use Rufus to flash to USB.

## Install Proxmox
> Repeat these steps for each node that will be running Proxmox.

Ensure in BIOS that CPU virtualization is enabled.

1. Boot into the USB
2. Select **Terminal Mode**
3. Use Australia/Sydney as timezone
4. Enter _short pass_ as root password 
5. Enter _personal email_ as admin email
6. Enter IP details according to IPAM
    1. Hostname: <node>.pve.jordanhome.dev
    2. DNS: 192.168.1.1
7. Select **Install & Reboot into Proxmox**


Validate that the Proxmox GUI is accessible at **IP:8006**


### No Valid Subscription Hook
Proxmox prompts you to add a valid license everytime you login.
Using the [PVE Nag Buster](https://github.com/foundObjects/pve-nag-buster/) we can remove this.
```
wget https://raw.githubusercontent.com/foundObjects/pve-nag-buster/master/install.sh
bash install.sh
```

After installing, reboot the server


## Proxmox Cluster Setup
We cluster each proxmox node in the network for easier administration.

### Setting up the first node in the clsuter
Follow below steps in the Proxmox GUI
1. Navigate to **Datacenter** > **Cluster**
2. Select **Create Cluster**
3. Enter _jordanhome_ as the name. Leave the other details as default. Select **Create**
4. Select **Join Information** and copy it somewhere. This is used for joining other nodes to the cluster

### Joining other nodes into the cluster
Follow below steps in the Proxmox GUI
1. Navigate to **Datacenter** > **Cluster**
2. Select **Join Cluster**
3. Paste the join information from below section. Select **Submit**

### Fixing certs between nodes
There is an issue between nodes in the cluster where they have invalid certs. Once all nodes in the network are joined, run the following on each one in the CLI:
```
pvecm updatecerts
pvecm updatecerts --force
```

Then reboot each one.


### Validation

Navigating to any of the Proxmox web GUIs should show all nodes in a list for the cluster. You should be able to see node details when selecting one.

## NAS Setup
The below steps will setup an NFS share on the cluster. This is used for VM backups and templates.

### On the NAS
Create the Shared folder on the NAS if it doesn't exist. **This should only need to be done once.**
1. Log into the NAS
2. Navigate to **Control Panel** > **Shared Panel** > **Create**
3. Enter below details:
    1. Name: Proxmox
    2. Description: Proxmox Backups/ISOs/Templates
    3. Give _admin_ and _jtwylde_ Read/Write permissions
4. Select **Create**
5. Edit the new Shared Folder
6. Navigate to **NFS Permissions** and select **Create**
7. For each node in the Proxmox cluster, do the following:
    1. Enter the IP
    2. Change Squash to **Map root to admin**
    3. Select **Save**
8. Select **Save**


### In the cluster
1. Navigate to **Datacenter (jordanhome)**. 
2. Select **Storage** > **Add** > **NFS**, and fill out below details
    1. ID: NAS
    2. Server: <NAS IP>
    3. Export: /volume1/proxmox
    4. Content: ISO, Container Template, Backup, Import
3. Select **Add**, and confirm that the NAS now shows available as storage for each node

