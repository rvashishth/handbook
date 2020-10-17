### Docker Build

Use current dir as context

```bash
$docker build .
```

Use a docker file from different location

```bash
$docker build -f /path/to/dockerfile .
```

Tag an image with repo and tag name

```bash
$docker build -t myrepo/myimage .
```

Tag multiple repo for single docker build

```bash
$docker build -t azure-acr/scg -t optum-repo/scg
```

### Docker run

Run a bash shell container in interactive mode, once run below will open a shell terminal from busybox

```bash
$docker run -it busybox sh
```

Docker run with local volume mount

```bash
docker run --rm -it --name azure-cli -v /Users/rvashish/intellijworkspaces/link/pulsar-perf/cert:/home/cert docker.repo1.uhc.com/microsoft/azure-cli /bin/bash
```