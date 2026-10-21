There is a single NAS on the network, a Synology Diskstation. Its primary purpose is to store the media library used in the [[Jellyfin|/apps/Jellyfin.md]] stack and backups for apps and VMs. 

The web portal for the NAS is accessible [[here|https://nas.jordanhome.dev]].

# Shared Folders
A shared folder is a folder on the NAS that can be exposed to the network (i.e over NFS). We define three shared folders on the NAS:

### Proxmox
Used for storing images/templates/snippets for the [[proxmox cluster|/build/Proxmox Build Guide.md]].

### JordanNAS
Stores the entire media library under `/JordanNAS/media-library` and any miscellaneous app assets under `/JordanNAS/app-assets`.

### Backups
Used for backups of app configs & VMs. App backups are found under `/Backups/config-backups` and VM backups are found under `/Backups/dump`. 
