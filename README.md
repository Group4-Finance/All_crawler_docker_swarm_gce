# 啟動 portainer
docker stack deploy -c portainer.yml por

# 啟動mysql
docker stack deploy --with-registry-auth -c mysql-swarm.yml mysql

# 啟動 rabbitmq (目前換成redis)
docker stack deploy --with-registry-auth -c rabbitmq-swarm.yml rabbitmq

# 啟動 redash 
docker stack deploy --with-registry-auth -c docker-compose-redash.yml redash

# 啟動 api 
docker stack deploy --with-registry-auth -c docker-compose-api-swarm.yml api
