In this practice, add (install) __numpy__ to existing Python container and create a new container image.

* Pick a Python container image from Docker Store or use already pulled one
* Add __numpy__ manually
* Create Dockerfile
* Build a new image
* Test

From Docker Store, I pick a Python image tag, 3.6.5-alpine3.4 (lite version).
```
$ docker pull python:3.6.5-alpine3.4
$ docker images
```
Test a pulled image.
```
$ docker run -ti --rm python:3.6.5-alpine3.4 /bin/sh
/ # python --version
Python 3.6.5
/ # pip --version
pip 9.0.3 from /usr/local/lib/python3.6/site-packages (python 3.6)
```

Install __numpy__
``` Install numpy using pip
$ pip install numpy
...
      File "numpy/core/setup.py", line 675, in get_mathlib_info
        raise RuntimeError("Broken toolchain: cannot link a simple C program")
    RuntimeError: Broken toolchain: cannot link a simple C program

    ----------------------------------------
Command "/usr/local/bin/python -u -c "import setuptools, tokenize;__file__='/tmp/pip-build-6gmc_3_v/numpy/setup.py';f=getattr(tokenize, 'open', open)(__file__);code=f.read().replace('\r\n', '\n');f.close();exec(compile(code, __file__, 'exec'))" install --record /tmp/pip-dd8ac3nu-record/install-record.txt --single-version-externally-managed --compile" failed with error code 1 in /tmp/pip-build-6gmc_3_v/numpy/
```

Search for resolutions:
> by *failed with error code 1 in /tmp/pip-build-6gmc_3_v/numpy/* - Not easy to find RESOLUTION
> by *Broken toolchain: cannot link a simple C program* 

Add (make automake gcc g++ subversion python3-dev) or (build-base) before installing __numpy__
``` add make automake gcc g++ subversion python3-dev
$ apk update
$ apk add make automake gcc g++ subversion python3-dev 

$ pip install numpy
...
Successfully built numpy
Installing collected packages: numpy
Successfully installed numpy-1.14.2
```

HINT: To verify __numpy__ installed.
``` Execute interatively
/ # python
>>> import numpy
>>> numpy.version.version
'1.14.2'

>>> print(numpy.__version__)
'1.14.2'
```
*OR*

``` Execute 
/ # python -c "import numpy; print(numpy.version.version)"
1.14.2
/ # python -c "import numpy; print(numpy.__version__)"
1.14.2
```
Creat a new Dockerfile based on manual process
```Dockerfile - Python with numpy installed based on alpine3.4
FROM python:3.6.5-alpine3.4

RUN apk update && apk add make automake gcc g++ subversion python3-dev
(OR)
RUN apk update && apk add build-base

RUN pip install numpy
```

Build a Python container image with __numpy__ installed.

``` New Python container image create and check
$ docker build -t python/numpy1.14.2:1.0 .
Step 1/3 : FROM python:3.6.5-alpine3.4
 ---> 5c527d46f668
Step 2/3 : RUN apk update && apk add build-base
 ---> Running in 8d06bf54ee84
 ...
Step 3/3 : RUN pip install numpy
 ---> Running in 31e95ff0e117
 ...
Successfully built b9784f4953ad
Successfully tagged python/numpy1.14.2:1.0

$ docker images
REPOSITORY           TAG                 IMAGE ID            CREATED             SIZE
python/numpy1.14.2   1.0                 b9784f4953ad        4 minutes ago       308MB

$ docker run -ti --rm python/numpy1.14.2:1.0 /bin/sh
/ # python -c "import numpy; print(numpy.version.version)"
1.14.2
```




