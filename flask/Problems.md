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