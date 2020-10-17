- [Dockerfile](#dockerfile)
- [Docker Build System](#docker-build-system)
- [Parser Directive](#parser-directive)
- [ENV ARG LABEL](#env-arg-label)
- [RUN ENTRYPOINT CMD](#run-entrypoint-cmd)
- [EXPOSE](#expose)
- [WORKDIR ADD COPY](#workdir-add-copy)
- [VOLUME](#volume)
- [Misc](#misc)

### Dockerfile

- [Dockerfile Reference](https://docs.docker.com/engine/reference/builder/)
- [Dockerfile Best Practice](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)

### Docker Build System

- docker build commands we can pass either PATH or GitHub url

- by default build command is run by docker daemon. we can use buildkit by simply setting env var DOCKER_BUILDKIT=1 before running docker build command

> Dockerfile and **.dockerignore** files are used by daemon or buildkit but never copied into image by ADD or COPY command

- moby/BuildKit is another build system like docker daemon. starting docker 18.09 docker supports new build system

- to improve build performance exclude files using .dockerignore [check rules here](https://docs.docker.com/engine/reference/builder/#dockerignore-file)

- we can tag same image into multiple repositories by passing multiple -t args. Check if - this is how we can push SCG image on Azure and Artifactory repo?

- build cache is only used for image which were locally build we can use —cache-from to use build cache of different image.

### Parser Directive

- first line of docker file, affect the way dockerfile is parsed. docker supports two parser directive syntax, escape

- syntax is by Moby/buildkit which allow us to use the dockerfile builder available as docker image without updating the docker daemon. Docker distributes official versions of the images that can be used for building Dockerfiles under docker/dockerfile repository on Docker Hub. Select image version carefully

- escape parser allow us to define which character to use as escape. Default is `\` .escape character `\` can be used within line and to escape to newline. Though escaping within RUN command line is not supported, only escape to new line works for RUN command

### ENV ARG LABEL

- ENV instruction `ENV name Rahul`, env can be read as either `$name` or `${name}` or `${name}_var`

- we can use ternary operators while resolving env variables - see dockerfile reference

- make sure to check if the instruction in which you are to use env variable supports it. Not all instructions does.

### RUN ENTRYPOINT CMD

### EXPOSE

### WORKDIR ADD COPY

### VOLUME

### Misc

- Alpine, Busybox linux don't have bash installed
- When you issue a docker build command, the current working directory in terminal is called the **build context**
