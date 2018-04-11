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

``` add make automake gcc g++ subversion python3-dev
$ apk update
$ apk add make automake gcc g++ subversion python3-dev 

$ pip install numpy
...
Successfully built numpy
Installing collected packages: numpy
Successfully installed numpy-1.14.2
```
Alternatively, add __build-base__ before pip install numpy. 
(*Adding __build_base__ requires creating a link of locale.h.*)
(*ln -s /usr/include/locale.h /usr/include/xlocale.h*)

Put together build a container image of Python on numpy
```Dockerfile - Python with numpy installed based on alpine3.4
FROM python:3.6.5-alpine3.4

RUN apk update && apk add make automake gcc g++ subversion python3-dev
(OR)
RUN apk update && apk add build-base
# RUN ln -s /usr/include/locale.h /usr/include/xlocale.h

RUN pip install numpy
```

HINT: To verify to check numpy installed.
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


