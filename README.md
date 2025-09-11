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

購買到網域後，要將IP掛上購買的網域，如Hinet 可從HiNet DNS代管進入到DNS記錄管理，將其掛上IP

### 使用cerbot申請憑證
sudo certbot certonly --standalone -d <yourdomain.com> -d <www.yourdomain.com>
或
sudo certbot --nginx -d <yourdomain.com>

指令執行後會出現一些文字需要複製，將這些文字複製到購買的網域的DNS記錄管理(如Hinet)，需要填入的值有Ldata和Rdata，記錄類型為TXT，紀錄完按儲存 

申請完憑證會得到四個檔案，只需要 privkey.pem 和 fullchain.pem 即可

將這兩個檔案複製到啟動api服務的遠端機器上，並事先在遠端機器上先建立資料夾以存放這兩個檔案

###在建立根目錄
sudo mkdir /certs

### 賦予權限
sudo chown kung:kung /certs

### 檢查是否有根目錄存在以及權限
ls -ld /certs

### 複製檔案到雲端機器
scp /mnt/c<privkey所在資料夾>/privkey.pem \<user name>\@<remote_server>:/certs/

scp /mnt/c<fullchain所在資料夾>/fullchain.pem \<user name>\@<remote_server>:/certs/

user name → 遠端機器的使用者名稱

remote_server → 遠端 IP 

### 確認certs內有無key
進入certs根目錄

cd /certs

查看目錄內檔案
ls

最後只要在原本啟動的api command最後加入以下指令即可完成ssl憑證以啟動htttps

--ssl-keyfile /<privkey所在資料夾>/privkey.pem --ssl-certfile /<fullchain所在資料夾>/fullchain.pem
