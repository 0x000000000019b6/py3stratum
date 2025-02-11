
# Description:

This is implementation of Stratum protocol for server and client side
using asynchronous networking written in Python Twisted.

Forked from: https://github.com/ahmedbodi/stratum

# Requirements:
**OS System(Tested):** Ubuntu 16.04, Ubuntu 18.04, Ubuntu 20.04, Windows 10

**Python:** 3.6+

# Installation (Ubuntu):

 - **From GIT, for developers:**
   
	``` git clone git://github.com/0x000000000019b6/py3stratum.git ```
   
	``` sudo apt-get install python3-dev ```
   
	``` cd py3stratum ``` 
   
	``` sudo python3 setup.py develop ``` 
    
	``` cp py3stratum/config_sample.py py3stratum/config.py ```
    
 - **From package, permanent install for production use:**
   
	``` git clone git://github.com/0x000000000019b6/py3stratum.git ```
   
	``` sudo apt-get install python3-dev ```
   
	``` sudo apt-get install python3-setuptools ```
   
	``` cd py3stratum ``` 
   
	``` sudo python3 setup.py install ``` 

	``` cp py3stratum/config_sample.py py3stratum/config.py ```

***Note:*** *Debian don't have a 'sudo' command, please do the installation
process as a root user.*

# Configuration:

 - **Basic configuration:**
Copy config_default.py to config.py
Edit at least those values: HOSTNAME, BITCOIN_TRUSTED_*

 - **Message signatures:**
For enabling message signatures, generate server's ECDSA key by
```python3 py3stratum/signature.py > signing_key.pem```
and fill correct values to SIGNING_KEY and SIGNING_ID (config.py)

- **Creating keys for SSL-based transports**
For all SSL-based transports (HTTPS, WSS, ...) you'll need private key
and certificate file. You can use certificates from any authority or you can
generate self-signed certificates, which is helpful at least for testing.

	Following script will generate self-signed SSL certificate:
	
	```
	#!/bin/bash
	openssl genrsa -des3 -out server.key 1024
	openssl req -new -key server.key -out server.csr
	cp server.key server.key.org
	openssl rsa -in server.key.org -out server.key
	openssl x509 -req -in server.csr -signkey server.key -out server.crt
	```

	Then you have to fill SSL_PRIVKEY and SSL_CACERT in config file with
	values 'server.key' and 'server.crt'.

# Startup:

 - **Start devel server:**
	```
	twistd -ny launcher.tac
	```
	
 - **Start devel server *without* lowlevel messages of Twisted:**
	```
	twistd -ny launcher.tac -l log/twistd.log
	```

***Note:*** *If you want to run it in the background you can remove the -ny and replace it with -y: ```twistd -y launcher.tac```*
