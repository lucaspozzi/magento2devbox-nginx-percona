sudo docker-compose down

sudo docker rmi $(sudo docker images -a -q)
sudo docker rm $(sudo docker ps -a -q)
docker images --no-trunc | grep '<none>' | awk '{ print $3 }' \
    | xargs -r docker rmi

sudo docker-compose up -d

sudo docker exec -ti magento_php_1 /bin/bash

apt-get update
apt-get install libxml2-dev
docker-php-ext-install soap

###NOTE#####
We need to learn how to build ourn own 'php' image.
If you execute DOWN, you'll have to install 'soap' manually again.