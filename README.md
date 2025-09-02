## 啟動 portainer
docker stack deploy -c portainer.yml por

## 啟動mysql
docker stack deploy --with-registry-auth -c mysql-swarm.yml mysql

## 啟動 rabbitmq (目前換成redis)
docker stack deploy --with-registry-auth -c rabbitmq-swarm.yml rabbitmq

## 啟動 redash 
docker stack deploy --with-registry-auth -c docker-compose-redash.yml redash

## 啟動 api 
DOCKER_IMAGE_FULL=qweasdkimo123/api:0.0.3 docker stack deploy --with-registry-auth -c docker-compose-api-swarm.yml api

## ssl憑證
!!申請憑證前要先購買或找到免費的網域

sudo certbot certonly --standalone -d <yourdomain.com> -d <www.yourdomain.com>
或
sudo certbot --nginx -d <yourdomain.com>

申請完憑證會得到四個檔案，只需要 privkey.pem 和 fullchain.pem 即可

將這兩個檔案複製到啟動api服務的遠端機器上，並事先在遠端機器上先建立資料夾以存放這兩個檔案

scp /<privkey所在資料夾>/privkey.pem <user>@<remote_server>:/certs/
scp /<fullchain所在資料夾>/fullchain.pem <user>@<remote_server>:/certs/

user → 遠端機器的使用者名稱

remote_server → 遠端 IP 

最後只要在原本啟動的api command最後加入以下指令即可完成ssl憑證以啟動htttps
--ssl-keyfile /<privkey所在資料夾>/privkey.pem --ssl-certfile /<fullchain所在資料夾>/fullchain.pem
