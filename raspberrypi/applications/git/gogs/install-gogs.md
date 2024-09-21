# README #

Installing gogs on raspberry pi 4

# Create docker volume.
$ docker volume create --name gogs-data

# Prepare git user for ssh access
sudo useradd git
sudo passwd git

ssh git@pi

sudo mkdir -p /home/git
cd /home/git
sudo ln -s /mnt/raid2/applications/gogs/git/.ssh .ssh
sudo chown -h git:git .ssh
sudo chmod 775 /mnt/raid2/applications/gogs

# Start gogs
docker run -d \
    --name gogs \
    -p 2222:22 -p 10080:3000 \
    -e PUID=1001 -e PGID=1001 \
    -v /mnt/raid2/applications/gogs/git:/data/git \
    -v /mnt/raid2/nodes/raspberrypi/configs/gogs/conf:/data/gogs/conf \
    -v /mnt/raid2/applications/gogs/gogs/data:/data/gogs/data \
    -v /mnt/raid2/applications/gogs/ssh:/data/ssh \
    gogs/gogs:0.13.0

## Upgrade gogs

Pull the latest gogs version:
`docker pull gogs/gogs:0.13.0`

Stop the running gogs instance:
`docker stop gogs`

Remove the old container:
`docker rm gogs`

Start a new gogs instance using the command in the `Start gogs` section.