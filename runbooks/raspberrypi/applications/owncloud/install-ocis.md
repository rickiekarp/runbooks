# README #

## Initialize owncloud infinite scale

docker run --rm -it -u root -v /mnt/raid2/applications/owncloud/config:/etc/ocis owncloud/ocis:2.0.0-linux-arm init

## Start owncloud infinite scale

docker run -d \
    --name ocis \
    -u root \
    --env-file="operation/ocis/env" \
    -v /mnt/raid2/applications/owncloud/config:/etc/ocis \
    -v /mnt/raid2/applications/owncloud/data:/var/lib/ocis \
    -p 9200:9200 \
    --add-host=ocis.rickiekarp.net:172.17.0.2 \
    owncloud/ocis:2.0.0-linux-arm64

## Upgrade owncloud

Pull the latest owncloud version:
`docker pull owncloud/ocis:2.0.0-linux-arm64`

Stop the running owncloud instance:
`docker stop ocis`

Remove the old container:
`docker rm ocis`

Start a new owncloud instance using the command in the `Start owncloud` section.

## Monitor upgrade

Check the logs:
`docker logs -f ocis`