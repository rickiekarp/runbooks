# README #

## Start nextcloud
<!-- docker run -d \
    --name ocis \
    -p 9200:9200 \
    -v /mnt/raid1/applications/owncloud:/var/tmp/ocis \
    owncloud/ocis:1.1.0-linux-arm64 -->


docker run -d \
    --name ocis \
    --env-file="operation/ocis/env" \
    -e PROXY_TLS=false \
    -p 9200:9200 \
    owncloud/ocis:1.0.0-linux-arm


docker run \
    --name ocis \
    --env-file="operation/ocis/env" \
    -e PROXY_TLS=false \
    -p 9200:9200 \
    owncloud/ocis:linux-arm