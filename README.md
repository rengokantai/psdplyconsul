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

run nginx via docker(ckeck prev course 35)
```
docker run -d --name web -p 8080:80 --restart unless-stopped -v /home/vagrant/ip.html:/usr/share/nginx.html/ip.html:ro nginx
```

lb:
```
/vagrant/provision/setup.lb.sh
/vagrant/provision/install.consul-template.sh
ip = $(ifconfig eth1 | grep 'inet addr' |awk '{print substr($2,6)}')
consul agent -advertise $ip -config-file /vagrant/common.json -config-file /vagrant/lb.service.json &
consul-template -config /vagrant/provision/lb.consul-template.hcl &
```

```
cp /vagrant/provision/haproxy.cfg /home/vagrant/.
docker run -d --name haproxy -p 80:80 --restart unless-stopped -v /home/vagrant/haproxy.cfg:/usr/local/etc/haproxy.cfg:ro haproxy:1.6.5-alpine
```
install config into kv for lb
```
curl -X PUT -d '4096' http://localhost:8500/v1/kv/prod/portal/haproxy/maxconn
curl -X PUT -d '5s' http://localhost:8500/v1/kv/prod/portal/haproxy/timeout-connect
curl -X PUT -d '50s' http://localhost:8500/v1/kv/prod/portal/haproxy/timeout-server
curl -X PUT -d '50s' http://localhost:8500/v1/kv/prod/portal/haproxy/timeout-client
curl -X PUT -d 'enable' http://localhost:8500/v1/kv/prod/portal/haproxy/stats
```
####10
(tbc)
