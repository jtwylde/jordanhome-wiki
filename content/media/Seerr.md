Seerr is used as a frontend for requesting movies and shows in the media stack.

## Restoring from backup
Follow below steps to restore from the automated Seerr backups in the NAS. Assumes access to the [[Kubernetes|/deploy/Kubernetes.md]] cluster.

1. Shut down Seerr

`sudo kubectl scale statefulset seerr -n jellyfin --replicas=0`

2. Create temporary pod

```bash
echo "apiVersion: v1
kind: Pod
metadata:
  name: seerr-maintenance
  namespace: jellyfin
spec:
  restartPolicy: Never
  containers:
    - name: maintenance
      image: busybox:1.36
      command: ["sh", "-c", "sleep 86400"]
      volumeMounts:
        - name: backup
          mountPath: /backup
        - name: seerr
          mountPath: /seerr
  volumes:
    - name: seerr
      persistentVolumeClaim:
        claimName: seerr-config
    - name: backup
      persistentVolumeClaim:
        claimName: media-config-backups-pvc" | sudo kubectl apply -f -
```

3. Exec into the pod

`sudo kubectl exec -it seerr-maintenance -n jellyfin -- sh`

4. Copy backup files across

```bash
# confirm latest backup
ls /backup/jellyseerr
cp /backup/jellyseerr/jellyseerr-<date>.tar.gz /tmp
cd /tmp
tar -xvf jellyseerr-<date>.tar.gz
cp settings.json /seerr/
cp db.sqlite /seerr/db

exit
```

5. Restart Seerr

`sudo kubectl scale statefulset seerr -n jellyfin --replicas=1`