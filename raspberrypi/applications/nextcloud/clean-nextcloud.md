# README #

## Enter container
docker exec -u www-data -it cloud bash

## Clean up old file versions
php occ versions:cleanup

## Clean up trash bin
php occ trashbin:cleanup --all-users

## The files:scan command scans for new files and updates the file cache
php occ files:scan --all
