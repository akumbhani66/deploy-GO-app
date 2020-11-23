# Deploy Go app for production

# Prerequisites
1. Ubuntu 18.04 server
2. GO - setup locally
3. Nginx - Revers-proxy/load-balancing
4. Domain - optional
5. Bonus - Free SSL 

In one of my GO project, i required to deploy app for production. There are
several ways to deploy GO app now a days.

Here's i will describe the way i followed to deploy app for prod and so far i haven't
got any complain.

For my app which contains REST apis, daily user traffic is arouind 100-250 and it have
third api integration.

Cloud provider is digitalocean. and i took 4 GB/ 80 GB SSD machine,
For database, Postgres - managed database service by Digitalocean.

step:1 - Install required tools in machine
 
## Nginx 
- [reference](https://www.nginx.com)

## Certbot
- [reference](https://certbot.eff.org/lets-encrypt/ubuntuxenial-nginx)

Step: 2 - `Deploying GO binary`

In this step, you will create a systemd unit file to keep your application running in the background even when a user logs out of the server. This will make your application persistent, bringing you one step closer to a production-grade deployment.

First, create a new file in /lib/systemd/system directory named appname.service.

`sudo vim /lib/systemd/system/appname.service`

Add following params,

```
[Unit]
Description=appname

[Service]
Type=simple
# Make sure to restart app in case of exception
Restart=always
# How long you want to wait after exception
RestartSec=25s

# Location of GO binary
ExecStart=/home/user/Project/GoAPP/binaryname

[Install]
WantedBy=multi-user.target
```

> Check status of app like you do for other services

1. `sudo service appname start` - To start app
2. `sudo service appname status` - To Check Status of app
3. `sudo service appname restart` - To Reload app

Step: 3 - Set up reverse proxy for your app with Nginx

```
server {
    server_name your_domain www.your_domain;

    location / {
        proxy_pass http://localhost:9990;
    }
}
```

Spet: 4 - Add SSL with LetsEncrypt

```
sudo certbot --nginx -d your_domain.com -d www.your_domain.com
```









