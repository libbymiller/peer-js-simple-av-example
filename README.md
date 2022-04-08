# peer-js-simple-av-example

WIP -

 * peer js server + express
 * with apache ssl config

# Apache SSL config

Follow letsencrypt to get a generated ssl apache config file

```
sudo a2ensite example.conf
sudo a2enmod rewrite proxy proxy_balancer proxy_wstunnel proxy_http
sudo service apache2 reload
sudo service apache2 restart
```

# Add certs

```
sudo apt install certbot
sudo apt install python3-certbot-apache
```

# get the cert  

`sudo certbot --apache -d myserver.example.com --post-hook "/usr/sbin/service apache2 restart"`

then edit to match example in this dir: example-le-ssl.conf and restart apache

# setup peer.js server with express

as per: https://github.com/peers/peerjs-server

in this checked out dir

```
npm install peer
sudo node server.js
```

and go to https://myserver.example.com/test.html

