In this practice, add (install) Num.py to existing Python container.
* Pull or use pulled python container
* Create a new Dockerfile
  * Try full Python container or lite container
* Test a new image

From Docker Store, I pick a Python image tag, 3.6.5-alpine3.4
``` Python image tag, 3.6.5-alpine3.4
$ docker pull python:3.6.5-alpine3.4
$ docker images
```
Test a pulled image.
``` Test python manuall
$ docker run -ti --rm python:3.6.5-alpine3.4 /bin/sh
/ # which python
/usr/local/bin/python
/ # python --version
Python 3.6.5
/ # exit
```

