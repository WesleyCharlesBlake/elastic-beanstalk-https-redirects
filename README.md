#  Elastic Beanstalk HTTPS and redirects 

This example shows how you can terminate HTTPS in your Elastic Beanstalk environment.

### Requirements
1. ACM certifacte to attach your EB Load Balancer
2. Update [https-lb-terminate.config](.ebextensions/https-lb-terminate.config) with your ACM ceritifacate ARN
3.  2. Update [https.config](.ebextensions/https.config) with your desired domain in both server blocks.
4.  Create an Elastic Beanstalk environment
5.  `$ eb create redirect-apex -p "64bit Amazon Linux 2017.09 v4.4.6 running Node.js"`

Or if you have an environment already created.

1.  `$ git add .`
2.  `$ git commint -m "terminate https! and redirect to one domain!"`
3.  `$ eb deploy`

> Note: this config is specific to Nodejs with an Nginx proxy configured. Feel free to change as required for your solution stack


### Testing that this works

```
# verify that http://WWW. goes to https://apex

$ curl -I http://www.mydomain.com
HTTP/1.1 301 Moved Permanently
Date: Wed, 04 Apr 2018 12:00:50 GMT
Location: https://mydomain.com/
Server: nginx/1.12.1
Content-Length: 185
Content-Type: text/html
Via: 1.1 iad6-proxy-5.amazon.com:80 (Cisco-WSA/10.5.1-296)
Connection: keep-alive

# verify that https://WWW. goes to https://apex
$ curl -I https://www.mydomain.com
HTTP/1.1 301 Moved Permanently
Date: Wed, 04 Apr 2018 12:00:50 GMT
Location: https://mydomain.com/
Server: nginx/1.12.1
Content-Length: 185
Content-Type: text/html
Via: 1.1 iad6-proxy-5.amazon.com:80 (Cisco-WSA/10.5.1-296)
Connection: keep-alive

# verify that http://APEX goes to https://apex
$ curl -I http://mydomain.com
HTTP/1.1 301 Moved Permanently
Date: Wed, 04 Apr 2018 12:00:50 GMT
Location: https://mydomain.com/
Server: nginx/1.12.1
Content-Length: 185
Content-Type: text/html
Via: 1.1 iad6-proxy-5.amazon.com:80 (Cisco-WSA/10.5.1-296)
Connection: keep-alive

# finally, verify that that https://apex repond 200 OK

$ curl -I https://mydomain.com
HTTP/1.1 200 OK
Content-Length: 16
Content-Type: text/html; charset=utf-8
Date: Wed, 04 Apr 2018 12:01:54 GMT
ETag: W/"10-8csYTHwr/fSeZx//reF9Ref6bTE"
Server: nginx/1.12.1
X-Powered-By: Express
Connection: keep-alive
```