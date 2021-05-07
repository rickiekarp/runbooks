# README #

## Start nextcloud
<!-- docker run -d \
    --name ocis \
    -p 9200:9200 \
    -v /mnt/raid1/applications/owncloud:/var/tmp/ocis \
    owncloud/ocis:1.5.0-linux-arm64 -->


docker run -d \
    --name ocis \
    --env-file="operation/ocis/env" \
    -p 9200:9200 \
    owncloud/ocis:1.5.0-linux-arm