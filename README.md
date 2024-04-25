# HTTPS certificates

Follow next steps to create HTTPS test certificates

1. Create test authority (CA) `test-ca` <br/>
```shell
openssl genrsa -out test-ca.key 2048
openssl req -x509 -new -nodes -key test-ca.key -sha256 -days 365 -out test-ca.crt
#Country Name (2 letter code) [AU]:MX
#State or Province Name (full name) [Some-State]:Mexico
#Locality Name (eg, city) []:City
#Organization Name (eg, company) [Internet Widgits Pty Ltd]:Ayan
#Organizational Unit Name (eg, section) []:Company
#Common Name (e.g. server FQDN or YOUR name) []:Ayan
#Email Address []:adanurbanreyes@ayan.com
```
2. Create test certificate `test-c` signed by our test authority `test-ca`
```shell
openssl genrsa -out test-c.key 2048
openssl req -new -key test-c.key -out test-c.csr
#Country Name (2 letter code) [AU]:MX
#State or Province Name (full name) [Some-State]:Mexico
#Locality Name (eg, city) []:City
#Organization Name (eg, company) [Internet Widgits Pty Ltd]:AyanDating
#Organizational Unit Name (eg, section) []:Section
#Common Name (e.g. server FQDN or YOUR name) []:AyanDating
#Email Address []:adanurbanreyes@ayan.com 

#Please enter the following 'extra' attributes
#to be sent with your certificate request
#A challenge password []:
#An optional company name []:AyanDating
openssl x509 -req -in test-c.csr -CA test-ca.crt -CAkey test-ca.key \
  -CAcreateserial -out test-c.crt -days 365 -sha256
```


# httpsToHttp config

This config create a listener for HTTPS calls on 127.0.0.1:4567 and redirect them to HTTP 127.0.0.1:4566.

> The configuration needs HTTPS certificates take a look of [HTTPS certificates](#HTTPS certificates) section<br/>
> The need of this configuration was dued to expose a localstack container on HTTPS<br/>
