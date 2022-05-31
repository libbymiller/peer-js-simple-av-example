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

# open server ports

https: tcp: 443 

webrtc tcp: 554, 1935, 8080-8084, 8443-8445, 8888, 9091, 30000-33000

webrtc udp: 1935, 30000-33000

# get the cert  

`sudo certbot --apache -d myserver.example.com --post-hook "/usr/sbin/service apache2 restart"`

then edit to match example in this dir: example-le-ssl.conf and restart apache.

idea is to proxy ssl to 8443 node server below, including wss

# setup peer.js server with express

as per: https://github.com/peers/peerjs-server

check out this repo

```
npm install peer
sudo node server.js
```

(sudo becuase of access to the ssl certs)

and go to https://myserver.example.com/test.html

# Paired bot and remote.html

peer.js requires one party to make the call and for that it needs to know the other one's peerid. For my purposes I can hardcode one id (mozbot1) and have remote.html call it. You can test with two windows open one with bot.html one with remote.html.

For the data examples you need to connect before sending a message. Only remote.html can initiate a call.

# Weirdnesses

I only really found 2, both with asymmetric - video one way and audio the other

 * unless you put constraints in, you get only audio both ways (https://github.com/peers/peerjs/issues/147#issuecomment-647498880)
 * The video is duplicated for remote

# multiplayer

Peer.js can do multiple peers, but you need to make pair by pair calls for each. I think.

There's an example in public/room-audio-multipeer-with-presence

# stun / turn server

You don't always need these but it's easy to install one:

    sudo apt-get -y install coturn

    sudo nano /etc/turnserver.conf

contents

    realm=coturn.myserver.example.com
    fingerprint
    listening-ip=0.0.0.0
    external-ip=<EXTERNAL_IP>/<INTERNAL_IP> #or just the external ip
    listening-port=3478
    min-port=10000
    max-port=20000
    log-file=/var/log/turnserver.log
    verbose
    user=something:something
    lt-cred-mech


sudo nano /etc/default/coturn

    TURNSERVER_ENABLED=1


OPEN PORT tcp:3478 (and udp?)

check:

    netstat -pnltu | grep 3478

and setup for peer.js is

    var peer = new Peer(
    {
      key: 'peerjs', host: 'myserver.example.com', path: "/",debug:3, port: 443, secure: true,
      config: {'iceServers': [
       { url: 'turn:myserver.example.com:3478?transport=udp', username: 'something', credential: 'something' },
       { url: 'turn:myserver.example.com:3478?transport=tcp', username: 'something', credential: 'something' }
      ]}
    });




