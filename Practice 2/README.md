In this practice, add (install) Num.py to existing Python container.
* Pull or use pulled python container
* Create a new Dockerfile
  * Try full Python container or lite container
* Test a new image

From Docker Store, I pick a Python image tag, 3.6.5-alpine3.4
```
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
We pulled a lean python image and try to install __numpy__ in it. 
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
1. by *failed with error code 1 in /tmp/pip-build-6gmc_3_v/numpy/* - No RESOLUTION
2. by *Broken toolchain: cannot link a simple C program* - 
```
$ apk update
$ apk add make automake gcc g++ subversion python3-dev 

$ pip install numpy
...
Successfully built numpy
```
Alternatively, add __build-base__ before pip install numpy.

Installing collected packages: numpy
Successfully installed numpy-1.14.2
```

