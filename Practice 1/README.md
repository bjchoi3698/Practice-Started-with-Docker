Get Started with Docker - Practice #1
Using "The Docker Store" (https://store.docker.com/)

You will find out a lot of docker images. Let's start with 'php' image for exercising.
1. Search "php"
2. Choose officical php image (https://store.docker.com/images/php)

There are a lot of supported tags available. For demonstration, use [apache (7.2/stretch/apache/Dockerfile)]

3. Verify pulled docker images 
  $ docker images
  Start a docker container (alpine:3.4) *interactively* (-ti) and then *remove after exit* (--rm). It gives back interactive shell prompt (/ #) and verify software installed randomly.
  ```
  $ docker run -ti --rm alpine:3.4 /bin/sh
  / # 
  / # git --version
  / # curl
  / # vim
  / # wget
  ...
  / # exit
  ```
  
  'alpine:3.4' container is no longer running because of __*--rm*__ in docker run.
  ```
  $ docker ps -a
  ```
  Create a Dockerfile containing
  
```Dockerfile
  FROM alpine:3.4
  MAINTAINER ...
  # Run a few package install for test purpose
  RUN apk update
  RUN apk add vim
  RUN apk add curl
```

  Build a docker image.   *( -t (tag) bj(provider)/alpine-new(image):1.0 (tag) . (from current directory) )*
  ```
  $ docker build -t bj/apline-new:1.0 .
  Sending build context to Docker daemon  10.75kB
  Step 1/5 : FROM alpine:3.4
   ---> c7fc7faf8c28
  Step 2/5 : MAINTAINER ...
   ---> Running in 5b324c4b6ed2
  Removing intermediate container 5b324c4b6ed2
   ---> 387a8c9a7a51
  Step 3/5 : RUN apk update
   ---> Running in e0ab24b0e059
  fetch http://dl-cdn.alpinelinux.org/alpine/v3.4/main/x86_64/APKINDEX.tar.gz
  fetch http://dl-cdn.alpinelinux.org/alpine/v3.4/community/x86_64/APKINDEX.tar.gz
  v3.4.6-295-ge132e3f [http://dl-cdn.alpinelinux.org/alpine/v3.4/main]
  v3.4.6-160-g14ad2a3 [http://dl-cdn.alpinelinux.org/alpine/v3.4/community]
  OK: 5973 distinct packages available
  Removing intermediate container e0ab24b0e059
   ---> 73f4209425c5
  Step 4/5 : RUN apk add vim
   ---> Running in 1851a87c3349
  (1/5) Installing lua5.2-libs (5.2.4-r2)
  (2/5) Installing ncurses-terminfo-base (6.0_p20171125-r0)
  (3/5) Installing ncurses-terminfo (6.0_p20171125-r0)
  (4/5) Installing ncurses-libs (6.0_p20171125-r0)
  (5/5) Installing vim (7.4.1831-r3)
  Executing busybox-1.24.2-r14.trigger
  OK: 37 MiB in 16 packages
  Removing intermediate container 1851a87c3349
   ---> 9abcf9e9b04d
  Step 5/5 : RUN apk add curl
   ---> Running in d6560336f36c
  (1/4) Installing ca-certificates (20161130-r0)
  (2/4) Installing libssh2 (1.7.0-r0)
  (3/4) Installing libcurl (7.59.0-r0)
  (4/4) Installing curl (7.59.0-r0)
  Executing busybox-1.24.2-r14.trigger
  Executing ca-certificates-20161130-r0.trigger
  OK: 38 MiB in 20 packages
  Removing intermediate container d6560336f36c
   ---> 3bfc847b6020
  Successfully built 3bfc847b6020
  Successfully tagged bj/alpine-new:1.0
  ```
  Check docker images.
  ```
  $ docker images
  REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
  bj/alpine-new       1.0                 3bfc847b6020        18 minutes ago      32.2MB
  ...
  
  $ docker images -a
  REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
  bj/alpine-new       1.0                 3bfc847b6020        19 minutes ago      32.2MB
  <none>              <none>              9abcf9e9b04d        19 minutes ago      30.6MB
  <none>              <none>              73f4209425c5        19 minutes ago      5.58MB
  <none>              <none>              387a8c9a7a51        19 minutes ago      4.82MB
    ...
  alpine              3.4                 c7fc7faf8c28        2 months ago        4.82MB
  ```
  
  With <none> REPOSITORY, it is an intermediate image that could be used as cache image for later build. 
  Check the SIZE changes over the different IMAGE ID.

  It is the way we could build a customized docker image (bj/alpine-new:1.0) from the base image (alpine:3.4).
  We can check if vim and curl were installed on a new image.
  
  ```
  $ docker run -ti --rm bj/alpine-new:1.0 /bin/sh
  / # curl
  / # vim
  ```
  
  Next exercise is to change package instal if we want 'git', but not 'vim'. Updated Dockerfile looks
  
  ```Dockerfile Updated: replaced vim with git
  FROM alpine:3.4
  MAINTAINER ...
  # Run a few package install for test purpose
  RUN apk update
  RUN apk add git
  RUN apk add curl
```
  
  Build a docker image. 
  
  ```
  $ docker build -t bj/alpine-new:1.0 
  ...
  Step 4/5 : RUN apk add git
   ---> Running in 6594f7d633bc
  (1/6) Installing ca-certificates (20161130-r0)
  (2/6) Installing libssh2 (1.7.0-r0)
  (3/6) Installing libcurl (7.59.0-r0)
  (4/6) Installing expat (2.2.0-r1)
  (5/6) Installing pcre (8.38-r1)
  (6/6) Installing git (2.8.6-r0)
  Executing busybox-1.24.2-r14.trigger
  Executing ca-certificates-20161130-r0.trigger
  OK: 22 MiB in 17 packages
  Removing intermediate container 6594f7d633bc
   ---> 9c3efcceb147
  Step 5/5 : RUN apk add curl
   ---> Running in f5dff7b66a33
  (1/1) Installing curl (7.59.0-r0)
  Executing busybox-1.24.2-r14.trigger
  OK: 22 MiB in 18 packages
  Removing intermediate container f5dff7b66a33
   ---> ccda6e61d80d
  Successfully built ccda6e61d80d
  Successfully tagged bj/alpine-new:1.0
  ```
  
  Verify the docker image again.
  ```
  $ docker images
  REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
  bj/alpine-new       1.0                 ccda6e61d80d        2 minutes ago       23.5MB
  <none>              <none>              3bfc847b6020        37 minutes ago      32.2MB
  ```
  
  In here, <none> is not cached image, but it is the previously built image, called dangling image supposed to be clean.
  
 ```
 $ docker images --filter "dangling=true"
  REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
  <none>              <none>              3bfc847b6020        40 minutes ago      32.2MB
 ```
    
  We can remove it as
  ```
  $ docker rmi $(docker images -q --filter "dangling:true")
  Deleted: sha256:3bfc847b602081075892585d8f0cfa2a44f73a65dcdf3619b4d9a4c73be8ee95
  Deleted: sha256:756a9ca24ca5006c555ae7cc7d914a89085d9adadd65ed78582da3ddeeaaa062
  Deleted: sha256:9abcf9e9b04d3affb1f033935111b22afa2de7cc2c159b3cf0adf75390c8e5d1
  Deleted: sha256:83d1844d37f945b5025c0c6d053a793a8ff4129b1a44a5f1dfc845dcf72a4585
  ```
  
  
  
  
  
  
  
