
### Install AWG kernel

https://github.com/amnezia-vpn/amneziawg-linux-kernel-module

Ubuntu
Open Terminal and proceed with following instructions:

(Optionally) Upgrade your system to latest packages including latest available kernel by running 
```
apt-get full-upgrade. After kernel upgrade reboot is required.
```

Ensure that you have source repositories configured for APT - run vi /etc/apt/sources.list and make sure that there is at least one line starting with deb-src is present and uncommented.

Install pre-requisites - run sudo apt install -y software-properties-common python3-launchpadlib gnupg2 linux-headers-$(uname -r).

Run sudo add-apt-repository ppa:amnezia/ppa.

Finally execute sudo apt-get install -y amneziawg.
```

### Build and push to GitHub multi-arch images

Create multi-arch builder - one time deal:

```aiignore
docker buildx create --name multiarch --driver docker-container
```

Build and push to GitHub Docker Repo for multiarch:

```aiignore
DATE_TAG=$(date +%Y%m%d)
INAME=ghcr.io/nbogol/docker-awg
docker buildx build --builder multiarch --platform linux/amd64,linux/arm64 -t ${INAME}:latest -t ${INAME}:${DATE_TAG} --push .
```


### Build and push to GitHub current architecture

This is a fork from the original repo https://github.com/AYastrebov/docker-amneziawg

Build:
```aiignore
DATE_TAG=$(date +%Y%m%d)
docker build -t ghcr.io/nbogol/docker-awg:${DATE_TAG} .
docker tag ghcr.io/nbogol/docker-awg:${DATE_TAG} ghcr.io/nbogol/docker-awg:latest
```

Push:
```aiignore
docker push ghcr.io/nbogol/docker-awg:${DATE_TAG}
```
```aiignore
docker push ghcr.io/nbogol/docker-awg:latest
```

Or use a script:
```aiignore
./build-push-to-ghcr.sh
```

### Build summary table

```aiignore
  ┌────────────────────────────────────────────────────────┬────────────────────────────────────────┐
  │                        Command                         │                 Result                 │
  ├────────────────────────────────────────────────────────┼────────────────────────────────────────┤
  │ docker build                                           │ current arch only                      │
  ├────────────────────────────────────────────────────────┼────────────────────────────────────────┤
  │ docker buildx build                                    │ current arch only                      │
  ├────────────────────────────────────────────────────────┼────────────────────────────────────────┤
  │ docker buildx build --platform linux/amd64,linux/arm64 │ both, requires docker-container driver │
  └────────────────────────────────────────────────────────┴────────────────────────────────────────┘
```
