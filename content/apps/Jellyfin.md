Jellyfin is our media server of choice. We support the Jellyfin server with a standard servarr stack which helps to collect media files onto the NAS.

## Backups
Backups of all configs are taken once a day at 3AM AEST everyday. These are backed up onto the NAS.
All of the services except for Jellyfin support automatic backups. Jellyfin supports manual backups through the GUI, these are copied by the Cron as well so any manual backups taken will end up on the NAS.