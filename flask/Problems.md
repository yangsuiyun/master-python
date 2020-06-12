**This document used to record QA during flask development**

# Net connectiong
## flask can't get right url_scheme after Nginx
* Issue detail: 

During deployment, we use Nginx to do revers poxy. I use flask_restplus to build a restful api server and try to use swagger module. 
But swagger ui can't show. 
Nginx and flask app deploied in different PC. 
Url is https, request between Nginx and flask is http.
* Root cause:
1. Swagger ui need to call /swagger.json during swagger html building. 
While flask app don't know real https request and wsgi.url_scheme is http request which is not right.

2. Swagger need to call /swaggerui/ which is not exposed to outside.

* Solution:
1. In flask
```python
from flask import Flask
from werkzeug.contrib.fixers import ProxyFix

app = Flask(__name__)
app.wsgi_app = ProxyFix(app.wsgi_app)
```
2. In Nginx
```editorconfig
location / {
    proxy_pass http://your_upstream/;
    proxy_set_header   Host             $host;
    proxy_set_header   X-Forwarded-Proto  https;
}

location /swaggerui/ {
       proxy_pass http://your_upstream/swaggerui/;
       proxy_set_header Host   $host;
       proxy_set_header X-Forwarded-Proto https;
    }
```

# Special exception
## Flask died and failed to write DB
* Issue detail

A flask application under uWSGI deploied in K8s. It crashed without any exception randomly after flak trying to write DB.
DB is postgresql.

* Root cause
A connectoin between postgresql and sqlalchemy is supported by both.
Connection live time in postgresql is three hours by default. Buth, Flask-sqlalchemy connection live time is different. 
Sometimes the connection is destoried in postgresql, otherwise flask don't know yet. And still use the old connection. 
There will be an error. No exception throwed out is weird.

* Solution

Set connection recycle time in a short time.
    SQLALCHEMY_POOL_RECYCLE=10
