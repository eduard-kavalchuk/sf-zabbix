## Basic Zabbix agent-server communication
### Installation and configuration of a Zabbix agent
Fetch a debian package of a Zabbix agent:
```
wget https://repo.zabbix.com/zabbix/5.2/debian/pool/main/z/zabbix-release/zabbix-release_5.2-1+debian10_all.deb
```
Install the package:
```
sudo dpkg -i zabbix-release_5.2-1+debian10_all.deb
sudo apt update
sudo apt install -y zabbix-agent
```
Start the server:
```
sudo systemctl enable zabbix-agent
```
Open port 10050 (a default port of Zabbix agent):
```
sudo ufw allow 10050
```
Make sure the port is being listened by an agent:
```
sudo apt install net-tools
netstat -nltp
```
Generate a ```psk``` key to encrypt communication between server and agent:
```
sudo openssl rand -hex 32 > key.psk
sudo mv key.psk /etc/zabbix/
```
Edit ```zabbix_agentd.conf```:
```
sudo vim /etc/zabbix/zabbix_agentd.conf
```
The following setting should be specified:
```
Server=0.0.0.0/0    # To allow requests from any IP
Hostname=<Hostname-of-a-machine-on-which-zabbix-agent-is-installed>
# ServerActive=127.0.0.1    # Should be commented out
TLSConnect=psk
TLSAccept=psk
TLSPSKIdentity=PSK 001
TLSPSKFile=/etc/zabbix/key.psk
```
Restart the agent:
```
sudo systemctl restart zabbix-agent.service
```
### Installation of a Zabbix server
Clone a repository of a zabbix docker:
```
git clone https://github.com/zabbix/zabbix-docker.git
```
Switch to a directory with the repository:
```
cd zabbix-docker/
```
Start ```docker-compose```:
```
sudo docker-compose -f docker-compose_v3_alpine_pgsql_local.yaml up -d
```
### Setup a server-agent connection
The server is available at ```localhost:80```. Set up host and encryption having screenshots as an example.
