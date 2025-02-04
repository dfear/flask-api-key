# flask-api-key
Simple API Key authN using decarators via HTTPS using TLS.

This is a proof of concept (POC) of a RESTful end point using Flask.
Flask uses TLS for HTTPS connections.
Sample self-signed certs and keys are provided.
As writen, you can send a GET and POST.

The POST is authenticated using an API Key as a parameter.
Where the key is stored in a file, so the key value is not stored in code.
Be sure to change the value in api.key when deploying.

##Installation
####Dependencies
1. Python 2 or 3
2. Flask
3. virtualenv

####Setup
```
$ cd flask-api-key
$ virtualenv venv
$ . venv/bin/activate
$ pip install -r requirements.txt
$ python3 goaway.py
 * Running on https://127.0.0.1:443/ (Press CTRL+C to quit)
```

####How to regenerate a self-signed SSL certificate.
```
$ openssl genrsa -des3 -out example.key 4096
$ openssl req -new -key example.key -out example.csr
$ cp example.key example.key.org
$ openssl rsa -in example.key.org -out example.key
$ openssl x509 -req -days 365 -in example.csr -signkey example.key -out example.crt
```

##GET Test
```
curl -k https://127.0.0.1
Hellow world!
```
Flask server output:
```
127.0.0.1 - - [23/Oct/2016 14:55:55] "GET / HTTP/1.1" 200 -
```

##POST Tests
###Key in header
```
$ curl -k -H "Content-Type: application/json" -H "x-api-key: eiWee8ep9due4deeshoa8Peichai8Eih" -X POST -d '{"username":"xyz","password":"xyz"}' https://127.0.0.1/json/
Posted JSON!
```
Flask server output:
```
127.0.0.1 - - [23/Oct/2016 14:55:26] "POST /json/ HTTP/1.1" 200 -
```

###Key in argument
```
$ curl -k -H "Content-Type: application/json" -X POST -d '{"username":"xyz","password":"xyz"}' https://127.0.0.1/json/?key=eiWee8ep9due4deeshoa8Peichai8Eih
Posted JSON!
```
Flask server output:
```
127.0.0.1 - - [23/Oct/2016 14:55:26] "POST /json/?key=eiWee8ep9due4deeshoa8Peichai8Eih HTTP/1.1" 200 -
```
