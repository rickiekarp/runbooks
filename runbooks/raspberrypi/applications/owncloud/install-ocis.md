# README #

## Start owncloud infinite scale

docker run -d \
    --name ocis \
    --env-file="operation/ocis/env" \
    -p 9200:9200 \
    --add-host=ocis.rickiekarp.net:172.17.0.1 \
    owncloud/ocis:1.18.0-linux-arm

## Upgrade owncloud

Pull the latest owncloud version:
`docker pull owncloud/ocis:1.18.0-linux-arm`

Stop the running owncloud instance:
`docker stop ocis`

Remove the old container:
`docker rm ocis`

Start a new owncloud instance using the command in the `Start owncloud` section.

## Monitor upgrade

Check the logs:
`docker logs -f ocis`