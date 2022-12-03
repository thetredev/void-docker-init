Build the image as follows:
```
$ git clone https://github.com/thetredev/void-docker-init.git
$ cd void-docker-init
$ docker build -t void-docker-init:runit-base runit/image
$ docker build -t void-docker-init:runit-test runit/test
```

Run the image:
```
$ docker run --rm -p 8080:80 void-docker-init:runit-test
```

Beware that your shell will seemingly hang after you `docker run` the image. The only way to kill it is via `docker rm -f <container id>`. To get around that, you could run it in detached mode:
```
$ docker run -d --rm -p 8080:80 void-docker-init:runit-test
```

Lastly, head to http://localhost:8080 and check that NGINX is running.
