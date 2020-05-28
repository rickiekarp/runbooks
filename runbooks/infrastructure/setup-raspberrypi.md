# How to set up the raspberrypi

1. Copy secrets to pi

rsync -rlvpt $HOME/git/secrets/raspberrypi/home/pi/ pi@192.168.178.200:~/

2. SSH into pi and clone the infrastructure projects

ssh pi@192.168.178.200

mkdir projects
cd projects
git clone git@git.rickiekarp.net:rickie/infrastructure-raspberrypi.git
git clone git@git.rickiekarp.net:rickie/module-deployment.git
