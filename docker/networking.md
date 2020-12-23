# Docker networking 

Check current setting with:
`docker network ls`

Types:
- `none`  - No access to the container from the outside world, or vice versa
- `host`  - Container shares the network of the host, for example container listening on port 80 will be available on port 80 on the host. This can cause conflict if you want another instance of the same
- `bridge`  - Docker creates a virtual private network with a bridge device. default 172.17.0.1/24

## CNI - Container networking interface

