# Nginx Reverse Proxy Set Up ( Multiple Server On The Same Port One IP )
Tested on ubuntu 20.4(LTS)
## Install nginx
    sudo apt update
    sudo apt install nginx
## Adjusting the Firewall
Before testing Nginx, the firewall software needs to be adjusted to allow access to the service. Nginx registers itself as a service with ufw upon installation, making it straightforward to allow Nginx access. 

List the application configurations that ufw knows how to work with by typing:

    sudo ufw app list
You should get a listing of the application profiles:

<img width="733" alt="Screen Shot 2023-07-03 at 14 19 10" src="https://github.com/go-julian/nginx-reverse-proxy/assets/130023825/f1712c6f-2a29-47fc-9037-f6272de1dc9d">

As demonstrated by the output, there are three profiles available for Nginx:

- Nginx Full: This profile opens both port 80 (normal, unencrypted web traffic) and port 443 (TLS/SSL encrypted traffic)
- Nginx HTTP: This profile opens only port 80 (normal, unencrypted web traffic)
- Nginx HTTPS: This profile opens only port 443 (TLS/SSL encrypted traffic)

It is recommended that you enable the most restrictive profile that will still allow the traffic you’ve configured. Right now, we will only need to allow traffic on port 80.

You can enable this by typing:

    sudo ufw allow 'Nginx HTTP'
You can verify the change by typing:

    sudo ufw status
<img width="733" alt="Screen Shot 2023-07-03 at 14 23 50" src="https://github.com/go-julian/nginx-reverse-proxy/assets/130023825/85f24e97-27ab-40b1-93da-69aed7458d4d">

## Checking your Web server
We can check with the systemd init system to make sure the service is running by typing:

    systemctl status nginx

<img width="744" alt="Screen Shot 2023-07-03 at 14 25 47" src="https://github.com/go-julian/nginx-reverse-proxy/assets/130023825/58d42239-6d35-49a2-82fd-255415d25d33">

Get your server’s IP address by typing:

    ip add show
or

    hostname -I
When you have your server’s IP address, enter it into your browser’s address bar:

    http://your_server_ip
    
You should receive the default Nginx landing page:

<img width="598" alt="Screen Shot 2023-07-03 at 14 28 18" src="https://github.com/go-julian/nginx-reverse-proxy/assets/130023825/c5469da8-5dd7-4d22-a464-7c9e2a05cffb">

## Setting nginx reverse proxy

Edit default by typing:

    cd /etc/nginx/sites-available/
    sudo nano default
Remove content and replace:

    server {
        listen 80;
        server_name your_domain_1.com;

        location / {
                proxy_pass http://your_server1_local_ip:port/;
        }
    }

    server {
        listen 80;
        server_name your_domain_1.com;

        location / {
                proxy_pass http://your_server2_local_ip:port/;
        }
    }

Change domain and server ip match with your. Save file and restart nginx:

    sudo systemctl restart nginx

All done, good luck !

# REFERENCE
[How To Install Nginx on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-20-04#step-1-installing-nginx)
