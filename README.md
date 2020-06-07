# psodo
Host Photostructure Server on a Digital Ocean droplet.

## Create droplet
- customize the values in user-data.txt to your needs

#### On Digital Ocean website:
- create a 1GB volume in the datacenter of your choice - this config volume will be for persistent storage (e.g., certificates)
- create another volume in the same datacenter - this photo library volume can be whatever size you want, it will host your photos and videos
- create a droplet | marketplace | docker
- choose the standard plan
- the $5 option (1GB RAM, 1CPU, 25GB disk, 1000GB xfer) provides adequate performance, but the $10 option (2GB RAM, 1CPU, 50GB disk, 2000GB xfer) performs better
- for block storage, click "add volume"
  - click "add existing"
    - click on your config volume
- choose the same datacenter your Photostructure library and config volumes are also in - they must be in same datacenter
- check "User Data" and paste in the contents of your customized user-data.txt
- authentication: make sure your laptop's key is selected (to SSH in as root w/out password)
- name your droplet

#### Quickly after droplet is created and is booting:
- attach floating IP (floating IP must be in same region your Photostructure library and config volumes are in) - you might need to obtain a floating IP before you can attach it, I don't remember
- attach your photo library volume
- to watch it install:
  - `ssh root@domain.com` (make sure your DNS record is set so domain.com points to your floating IP, otherwise just ssh to the droplet's IP)
  - `tail -f /var/log/cloud-init-output.log`

#### After cloud-init finishes:
- `chown 1000 /storage/photos` (You only need to do this once, ever: the mount-pount of your photo library volume needs to be owned by UID that syncthing runs as (and hence also the UID of your one sftp user), but once you set it, it's in the inode for the directory (i.e., the owner stays with the directory wherever it is mounted.)

## Syncthing:
To connect, first start an ssh tunnel:
- `ssh -L 8384:127.0.0.1:8384 root@<your droplet's FQDN or IP>`
- `https://127.0.0.1:8384/`

## Photostructure:
- on first install and launch of PS webpage, select "No thanks, I like my photos and videos where they already are" and click "Start"
