---
title: Flask with uwsgi
date: 2019-07-16 14:49:20
tags:
---

## Should we use uwsgi with Flask?

Every time when we run a flask app using the internal server, we are prompted to not use that server in production. I believe the warning is based on performance, but what's the actually differences between using the build-in server and wsgi such as uwsgi? Let's have a look.

First, let's talk about multiple requests handling. Before I write this blog, I thought with the build-in server of flask, it can only handle one request per time. However, when I tested it with Locust, it can handle multiple request very well. The tested API is only a sleep function. So by default, the build in server enable threads to support multiple requests. 

```python
# by default, this will enable multi-threads support
app.run(host='0.0.0.0', debug=False, port=80)
# to disable it
app.run(host='0.0.0.0', debug=False, port=80, threaded=False)
# note: when debug is True, any modification of the app will make the app get reloaded.
```

So if our API is something like `sleep` fuction, only using the build-in server can satisfy us. However, in reality, most API will both have cpu-consuming tasks and io-waiting tasks. 
Thus in theory, assigning multiple requests to one flask app (with multiple threads) can not take advantage of multiple cores due to python's global GIL (global interprete lock).

## The advantage of using uwsgi

> 1. It can scale your APP automatically. 

With uwsgi, you can set the minimum and maximum number of works to handle requests. When too many requests come in simultaneously, uwsgi would spawn extra processes to process these requests.

> 2. With Uwsgi, you can take advantages of multiple cores.

If your app is computation heavy tasks (cpu intensive tasks) and you do not enable threads supporting in uwsgi configuration, each request will be processing in seperate process. 


## Using uwsgi compared with multi APP instances

By saying Multi app instances, I mean running multiple APP in different IP/Ports and using Nginx for load balancer. Running Multiple Apps has several constraints:

1. You have to decide how many instances to run. It cannot scale automatically.
2. With `threaded support`, you are risking of handling multple requests in the same processes. This will compromise the performance. Without `threaded support`, you may have a longer response time since the next request won't be processing until the current one finished. The largest number of requests you can handle simultaneously won't be greater than the number of app instances.

## Experiment

The best way to verify a theory is by testing. 

### Test with IO heavy task (threads enabled)
```python
import time

@app.route('/')
def hello():
    # sleep for two seconds
    time.sleep(2)
    return "Hello world from flask"
```
Then use `Locust` for testing. User number is 20, waiting time is 0.

![upstream](/images/flask_with_uwsgi/multi_flask_in_different_ports.png "running multiple app and load balancing them with nginx ")
![upstream](/images/flask_with_uwsgi/uwsgi_with_threads_supported.png "uwsgi with threads enabled")

### Test with CPU intensive task (threads enabled)

```python
import numpy as np
import time

@app.route('/')
def hello():
    np.random.random((40000, 3000))
    time.sleep(2)
    return "create a shape of (40000, 3000) numpy array"
```
NOTE: the average time cost for this function is `11000 ms` in this specific settings.

Then use `Locust` for test. User number is 10, waiting time is 0.

![upstream](/images/flask_with_uwsgi/multi_flask_in_3_ports_with_threads_enabled.png "running three instance with threads enabled")

![upstream](/images/flask_with_uwsgi/uwsgi_threads_disabled_cpu_intensive_task.png "uwsgi with threads disabled")


## Additional settings

### nginx load balancing
```nginx
upstream backend{
    server 0.0.0.0:81;
    server 0.0.0.0:82;
    server 0.0.0.0:83;
}

server {
    listen 80;
    location / {
        proxy_pass http://backend;
    }
}
```

### uwsgi configuration
```uwsgi
[uwsgi]
module = main
callable = app
# the minimum number of workers to keep all the time
cheaper = 2
# the maximum number of workers to spawn
workers = 20
# enable threads and set the threads as 5
threads = 5

socket = /tmp/uwsgi.sock
chown-socket = nginx:nginx
chmod-socket = 664
hook-master-start = unix_signal:15 gracefuulay_kill_them_all
```
