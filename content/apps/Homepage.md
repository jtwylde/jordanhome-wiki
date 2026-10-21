Homepage is a central homepage for the homelab (imagine the landing page for an intranet). It can be found [[here|https://homepage.jordanhome.dev]].

The configs for the homepage are defined in the [[Ansible|/infra/Ansible.md]] playbooks.

# Assets
The way I've configured Homepage requires a number of image assets (wallpaper, icons, etc) to be mounted into the container. The assets themselves loaded onto the [[NAS|/infra/NAS.md]], under `/JordanNAS/app-assets/homepage`. We then mount this using a pvc into the container.

# References
[[Homepage|https://gethomepage.dev]]