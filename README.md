## psdplyconsul
####5 Launching starting point
5 servers
```
consul-server
lb
web1
web2
web3
```
we use vagrant.step:  
consul-server
```
vagrant ssh consul-server
consul-server > consul agent -dev -advertise consul-serverip -client 0.0.0.0
```
test,visit
```
consul-serverip:8500/ui
```
webX:
```
vagrant ssh webx
```
get ip
```
ip = $(ifconfig eth1 | grep 'inet addr' |awk '{print substr($2,6)}')
consul agent -advertise $ip -config-file /vagrant/common.json -config-file /vagrant/web.service.json &
```

