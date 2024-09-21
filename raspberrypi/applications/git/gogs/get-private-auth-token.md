# Set up git to use gogs auth token

1. Generate new auth token in gogs settings
2. Configure token as seen below

## Configure Git to use auth token
git config \
  --global \
  url."http://${user}:${personal_access_token}@git.rickiekarp.net".insteadOf \
  "http://git.rickiekarp.net"
