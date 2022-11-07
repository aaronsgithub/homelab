# Aaron's Homelab
## Features

Most of the homelab services exist within a Kubernetes cluster. We have kept an NFS server and DNS server outside the cluster.
### Kubernetes

Each heading below represents an ArgoCD ApplicationSet which handles the deployment of the items listed under that heading based on the manifests contained in this repository.


- [x] = complete
- [ ] = to do

**System Services**
- [x] nfs-csi - to allow connection to NFS server providing default storage for persistent volume claims (allowing Pods to be moved across nodes)
- [x] metallb - a bare metal loadbalancer for Kubernetes
- [x] cert-manager - simplifies management of TLS certificates to secure communication with / between Kubernetes pods
- [x] metrics-server - gathers container resource metrics and enables Kubernetes autoscalling features.
- [x] ArgoCD - CD platform used to manage the deployment of the cluster itself as well as developer projects.
- [ ] Hashicorp Vault - secrets management
- [ ] Traefik - Kubernetes Ingress controller and reverse proxy
- [ ] Prometheus - monitor cluster metrics and provide alerts
- [ ] Grafana - dashboards for cluster metrics
- [ ] Authentik - Identity Provider for authentication and authorisation


**Developer Tools**
- [ ] gitea - self hosted git repository
- [ ] harbor - self hosted container registry
- [ ] JupyterHub - self hosted notebook server
- [ ] Datasette - a data exploration and publishing tool

**Applications**
- [ ] paperless-ngx - document management system
- [ ] immich - photo / image management system
- [ ] Overleaf - LaTex editor, for maths :)
- [ ] Invoice Ninja - for getting paid :)


## Architecture

The homelab consists of the following hardware.

**Lenovo M720q small form factor PC**

- 1TB - for Network storage (used by the nfs-csi Kubernetes storage backend)
- Intel i5-8500T processor with 6 cores
- 1TB - for Network storage (used by the nfs-csi Kubernetes storage backend)
- 237GB NVMe storage - for OS / System storage
- 1TB - for Network storage (used by the nfs-csi Kubernetes storage backend)

This device has Proxmox VE as a host operating system which makes it convenient to create Virtual Machines and Virtual Networks which makes it ideal for prototyping a multi node Kubernetes cluster on a single device.

On Proxmox, the following guest virtual machines have been configured:
- debian-11 - Running an NFS server on the local network
- k3s-server-01 - Kubernetes control plane node runnning K3S Kubernetes distribution, installed on debian-11
- 3 x k3s-agent-0[1-3] - Kubernetes worker notes, again running K3S on debian-11

**Raspberry Pi Model 3B**

We have pihole running on a Raspberry Pi 3B providing local DNS to the Traefik reverse Proxy.
I decided to keep this outwith the Kubernetes cluster so that other users' internet access is not affected by my experiments.

The ISP provided router did not allow local DNS configuration.


**Provisioning Notes**

At the moment, both devices were provisioned and hardened by hand. In future updates, I will look at explore provisioning with Terafform / Ansible to achieve the following goals:
- [ ] self bootstrapping cluster from OS installation to working cluster
- [ ] ability to spin up dev / test clusters so future development of the cluster can be done on separate VMs / a different node.