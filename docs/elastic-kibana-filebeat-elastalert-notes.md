### Run Elasticsearch & Kibana as docker containers
```
git clone https://github.com/justmeandopensource/elk
cd elk/docker
mv docker-compose-v7.1.1.yml docker-compose.yml
sudo sysctl -w vm.max_map_count=262144
docker-compose up -d
```
### Install Filebeat on client machine (Ubuntu 18.04)
```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
sudo apt-get install apt-transport-https
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
sudo apt-get update && sudo apt-get install filebeat
```
### Configure Filebeat and enable system module
Edit filebeat configuration and update kibana and elasticsearch url
```
sudo vi /etc/filebeat/filebeat.yml
```
```
sudo filebeat modules enable system
sudo filebeat test config
sudo filebeat test output
sudo filebeat setup
sudo systemctl start filebeat
```
### Install and setup elastalert
```
sudo apt-get install -y python
sudo apt-get install -y python-pip python-dev libffi-dev libssl-dev
git clone https://github.com/Yelp/elastalert.git
cd elastalert
sudo pip install "setuptools>=11.3"
sudo pip install pyOpenSSL
sudo python setup.py install
sudo pip install "elasticsearch>=5.0.0"
cp config.example.yaml config.yaml
```
Edit config.yaml and update es_host with IP address or dns name of the elasticsearch server
```
elastalert-create-index
```
Configure example_rules/example_frequency.yaml
### Testing a rule
```
elastalert-test-rule --config config.yaml example_rules/example_frequency.yaml
```
### Running elastalert
```
python -m elastalert.elastalert --verbose --rule example_frequency.yaml
```

## Postfix Gmail SMTP
Enable 2-factor authentication and Generate app password

### Install Postfix
```
sudo apt-get install postfix mailutils libsasl2-2 ca-certificates libsasl2-modules
```

### Postfix configuration to add
```
relayhost = [smtp.gmail.com]:587
smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
smtp_sasl_security_options = noanonymous
smtp_tls_CApath = /etc/ssl/certs
smtpd_tls_CApath = /etc/ssl/certs
smtp_use_tls = yes
```
```
cat /etc/postfix/sasl_passwd
[smtp.gmail.com]:587	server.jbs@gmail.com:qpdm vkxv hdpc boja
sudo chmod 400 /etc/postfix/sasl_passwd
sudo postmap /etc/postfix/sasl_passwd
sudo systemctl restart postfix
```
```
echo "Testing" | mail -s "Test Email" server.jbs@gmail.com
sudo postqueue -p
```
