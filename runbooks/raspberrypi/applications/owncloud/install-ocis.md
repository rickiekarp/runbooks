# README #

## Start owncloud infinite scale

docker run -d \
    --name ocis \
    --env-file="operation/ocis/env" \
    -p 9200:9200 \
    --add-host=ocis.rickiekarp.net:172.17.0.1 \
    -v /mnt/raid1/applications/owncloud/config:/config \
    -v /mnt/raid1/applications/owncloud/ocis:/var/tmp/ocis \
    owncloud/ocis:1.7.0-linux-arm