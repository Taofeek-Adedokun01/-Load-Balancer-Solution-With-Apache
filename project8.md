## Configure Apache As A Load Balancer

## Create an Ubuntu Server 20.04 EC2 instance and name it Project-8-apache-lb

`sudo apt update`

`sudo apt install apache2 -y`

`sudo apt-get install libxml2-dev`

## Enable following modules:

`sudo a2enmod rewrite`

`sudo a2enmod proxy`

`sudo a2enmod proxy_balancer`

`sudo a2enmod proxy_http`

`sudo a2enmod headers`

`sudo a2enmod lbmethod_bytraffic`

`sudo systemctl restart apache2`

`sudo systemctl status apache2`

## Configure load balancing

`sudo vi /etc/apache2/sites-available/000-default.conf`

`<Proxy "balancer://mycluster">
               BalancerMember http://<WebServer1-Private-IP-Address>:80 loadfactor=5 timeout=1
               BalancerMember http://<WebServer2-Private-IP-Address>:80 loadfactor=5 timeout=1
               ProxySet lbmethod=bytraffic
               # ProxySet lbmethod=byrequests
        </Proxy>

        ProxyPreserveHost On
        ProxyPass / balancer://mycluster/
        ProxyPassReverse / balancer://mycluster/`

`sudo systemctl restart apache2`

`sudo tail -f /var/log/httpd/access_log`

![Load-balancing-website](/images/load-balancer-website.PNG)

![Load-balancer-configuration](/images/Load-balancing-configuration.PNG)

![Load-balancer](/images/Load-balancer.PNG)

![Logs](/images/logs.PNG)