# Ingress in Kubernetes

Routes traffic based on the URL path (or domain) and manage SSL. It's basically a L7 load-balancer.  
You still need to expose the ingress with a service to make it accessible (high port)

Typical ingress deployments (ingress controller):
- nginx
- GCE
- haproxy
- traefik

Rules are called `ingress resources`.

## Ingress controller

