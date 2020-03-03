# wtfaid

## Create droplet
#### on DO website: create droplet | marketplace | docker
- standard plan
- $5 option (1GB RAM, 1CPU, 25GB disk, 1000GB xfer)
- add block storage
  - add existing
    - click on config volume
- choose datacenter NY 3
- check User Data and paste in user-data.txt contents
- authentication: make sure your laptop's key is selected (to SSH in as root w/out password)
- name your droplet

#### Quickly after droplet is created and is booting:
- attach floating IP
- attach photo library volume
- `ssh root@domain.com` (make sure your DNS record is set so domain.com points to your floating IP, otherwise just ssh to the IP address of the droplet)
- tail -f /var/log/cloud-init-output.log (to watch it install)

#### After droplet boots:
- chown 1000 /storage/photos (you only need to do this once, ever: /storage/photos needs to be owned by UID that syncthing runs as (and hence also the UID of your one sftp user), but once you set it, it's in the inode for the directory (i.e., the owner stays with the directory wherever it is mounted)


## Syncthing:
to connect, first start an ssh tunnel:
- ssh -L 8384:127.0.0.1:8384 root@<your droplet's IP>
- `https://127.0.0.1:8384/`


## Photostructure:
- on first install and launch of PS webpage, select "No thanks, I like my photos and videos where they already are" and click "Start"
