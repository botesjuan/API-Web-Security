# API Discovery  

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

# reboot kali  

sudo apt install --reinstall mitmproxy

sudo apt install golang-go
```  

>[Setting up Kali Linux and More for API course](https://university.apisec.ai/products/api-penetration-testing/categories/2150251486/posts/2157710611)  

>Add new Kali Linux user, add new user to sudo privilege groups and change new user shell to use:  

```bash
sudo useradd -m hapihacker
sudo usermod -a -G sudo hapihacker
sudo chsh -s /bin/zsh hapihacker
sudo passwd hapihacker
```  

>jwt tool install:  

```
sudo git clone https://github.com/ticarpi/jwt_tool
```  




