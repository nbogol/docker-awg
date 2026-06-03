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
