# antimonopoly.club

this is a static site that runs on an almalinux server, served by nginx.

## setup

### Nginx and HTTPS Setup

1. Link nginx config:
```
sudo ln -fs /var/www/antimonopoly.club/nginx/antimonopoly.club.bootstrap /etc/nginx/conf.d/antimonopoly.club.bootstrap

# ensure nginx config context is httpd_config_t
sudo chcon -t httpd_config_t /etc/nginx/conf.d/antimonopoly.club.conf
sudo semanage fcontext -a -t httpd_config_t "/etc/nginx/conf.d(/.*)?"
sudo restorecon -Rv /etc/nginx/conf.d

sudo service nginx reload
```

2. Set up HTTPS with Let's Encrypt:
```
# ensure webroot context is httpd_sys_content_t
sudo semanage fcontext -a -t httpd_sys_content_t "/var/www/antimonopoly.club(/.*)?"
sudo restorecon -Rv /var/www/antimonopoly.club

sudo certbot certonly --force-renewal -a webroot -w /var/www/antimonopoly.club -d antimonopoly.club -w /var/www/antimonopoly.club -d www.antimonopoly.club

sudo ln -fs /var/www/antimonopoly.club/nginx/antimonopoly.club /etc/nginx/conf.d/antimonopoly.club.conf

sudo service nginx reload
```

This will:
1. Set up the initial nginx config with the bootstrap file
2. Configure SELinux contexts properly
3. Obtain SSL certificates
4. Switch to the main nginx config with HTTPS enabled

