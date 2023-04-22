# README #

## Start jenkins
docker run -d \
    --name jenkins \
    -p 8081:8080 \
    -p 50000:50000 \
    -v /mnt/raid1/applications/jenkins:/var/jenkins_home \
    jenkins/jenkins:2.399-jdk17

## Upgrade jenkins

Pull the latest jenkins version:
`docker pull jenkins/jenkins:2.399-jdk17`

Stop the running jenkins instance:
`docker stop jenkins`

Remove the old container:
`docker rm jenkins`

Start a new jenkins instance using the command in the `Start jenkins` section.

## Monitor upgrade

Check the logs:
`docker logs -f jenkins`