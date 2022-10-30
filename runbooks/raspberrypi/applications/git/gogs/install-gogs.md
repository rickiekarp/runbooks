# README #

Installing gogs on raspberry pi

# Create docker volume.
$ docker volume create --name gogs-data

# Prepare git user for ssh access
sudo useradd git
sudo passwd git

ssh git@pi

sudo mkdir -p /home/git
cd /home/git
sudo ln -s /var/lib/docker/volumes/gogs-data/_data/git/.ssh .ssh
sudo chown -h git:git .ssh
sudo chmod 775 /var/lib/docker/volumes/gogs-data

# Start gogs
docker run -d \
    --name gogs \
    -p 2222:22 -p 10080:3000 \
    -e PUID=1001 -e PGID=1001 \
    -v gogs-data:/data \
    gogs/gogs:0.12.10

## Upgrade gogs

Pull the latest nextcloud version:
`docker pull gogs/gogs:0.12.10`

Stop the running gogs instance:
`docker stop gogs`

Remove the old container:
`docker rm gogs`

Start a new gogs instance using the command in the `Start gogs` section.