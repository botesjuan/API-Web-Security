# API Discovery  

>APISEC  

----  

## Setup Requirements  

>Lab setup require [mitmproxy](https://www.kali.org/tools/mitmproxy/) and Postman.  
>Import Certificates for Burp Suite and MitmProxy:  

![firefox-proxy-certificates-imported.png](/images/firefox-proxy-certificates-imported.png)  

>Install `Postman` and MitmProxy, `mitmproxy2swagger`:  

```sh
sudo wget https://dl.pstmn.io/download/latest/linux64 -O postman-linux-x64.tar.gz && sudo tar -xvzf postman-linux-x64.tar.gz -C /opt && sudo ln -s /opt/Postman/Postman /usr/bin/postman

sudo apt install git
sudo apt install docker-compose
sudo apt install docker.io

sudo apt-get remove mitmproxy
sudo apt-get purge mitmproxy
sudo apt update && sudo apt upgrade -y

# reboot Kali  

sudo apt install --reinstall mitmproxy

sudo apt install golang-go
```  

>[Setting up Kali Linux and More for API course](https://university.apisec.ai/products/api-penetration-testing/categories/2150251486/posts/2157710611)  

>Start `mitmproxy` and set Firefox to use foxyproxy 8080, same as burp, and install certificates for mitmproxy.  

----  

>Add new Kali Linux user, add new user to sudo privilege groups and change new user shell to use:  

```bash
sudo useradd -m hapihacker
sudo usermod -a -G sudo hapihacker
sudo chsh -s /bin/zsh hapihacker
sudo passwd hapihacker
```  

>jwt tool install:  

```
cd /opt
sudo git clone https://github.com/ticarpi/jwt_tool
cd jwt_tool

python3 -m pip install termcolor cprint pycryptodomex requests

sudo chmod +x jwt_tool.py
sudo ln -s /opt/jwt_tool/jwt_tool.py /usr/bin/jwt_tool
```  

>Start JWT tool with command `jwt_tool`.  

>Install KiteRunner:  

```
cd /opt
sudo git clone  https://github.com/assetnote/kiterunner.git
cd kiterunner
sudo make build
sudo ln -s /opt/kiterunner/dist/kr /usr/bin/kr
```

>Run kiterunner with command `kr`  

----  

## API Labs  

>[Your API Hacking Lab](https://university.apisec.ai/products/api-penetration-testing/categories/2150251486/posts/2157710632)  

>The Completely Ridiculous API (crAPI) from OWASP  

```
mkdir ~/lab 
cd ~/lab
sudo curl -o docker-compose.yml https://raw.githubusercontent.com/OWASP/crAPI/main/deploy/docker/docker-compose.yml
sudo docker-compose pull
sudo docker-compose -f docker-compose.yml --compatibility up -d
```  

![crapi-docker-lab.png](/images/crapi-docker-lab.png)  

>Validate if ***crAPI*** is working: `http://127.0.0.1:8888` and mail service on `http://127.0.0.1:8025`  

![crapi-owasp.png](/images/crapi-owasp.png)  

----  

>Setup and install vAPI  

```
cd ~/lab
sudo git clone https://github.com/roottusk/vapi.git
cd /vapi
sudo docker-compose up -d
```  

>Check status of running docker container instances: `docker ps -a`  
>Stop docker: `sudo docker-compose stop`  
>Restart docker if crapi-community unhealthy fail to start:  

```
sudo docker-compose restart
```  

>Navigate to `http://127.0.0.1/vapi` to get to the vAPI home page.  
>vAPI comes with a prebuilt Postman collection and environment.  
>Get JSON files from the vAPI/postman folder.  

>HackTheBox (Retired Machines) Lab Machines:  

* Craft
* Postman
* JSON
* Node
* Help  

----  

