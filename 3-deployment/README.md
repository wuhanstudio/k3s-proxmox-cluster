## K3S Applications

### 1. MetalLB

We first install the [Load Balancer](https://metallb.universe.tf/installation/)

```
# On your local PC
$ kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.13.7/config/manifests/metallb-native.yaml

# Please change the IP pool in the YML file
$ kubectl apply -f 3-deployment/1-metallb/metallb-configs.yml
```

### 2. Traefik

We use Traefik as the ingress controller. The documentation is available [here](https://doc.traefik.io/traefik/getting-started/quick-start-with-kubernetes/).

```

$ cd k3s-proxmox-cluster/3-deployment/2-traefik
$ kubectl apply -f 00-role.yml \
                -f 00-account.yml \
                -f 01-role-binding.yml \
                -f 02-traefik.yml \
                -f 02-traefik-services.yml

# You can change the domain name in YML file
$ kubectl apply -f 03-whoami-app.yml \
                -f 03-whoami-ingress.yml \
                -f 03-hello-world.yml \
                -f 03-hello-ingress.yml
```

To visit the website, add following settings to `hosts`:

```
# The ip address of your master node
192.168.1.130 hello.wuhanstudio.local
192.168.1.130 whoami.wuhanstudio.local
```

### 3. Cert-Manager

Let's generate TLS certificate using [cert-manager](https://cert-manager.io/docs/installation/).

```
$ cd k3s-proxmox-cluster/3-deployment/3-cert-manager

$ kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.10.0/cert-manager.yaml

# Use cmctl to verify the API installation
$ cmctl check api 

$ kubectl apply -f 04-cert-manager-config.yml
```

Now, we can generate [TLS certificates](https://cert-manager.io/docs/configuration/selfsigned/) for our applications.

```
$ kubectl apply -f 05-hello-tls-ingress.yml \
                -f 05-whoami-tls-ingress.yml
```

You can verify the certificates:

```
$ kubectl get pods --namespace cert-manager
$ kubectl describe secret hello-tls
$ kubectl describe secret whoami-tls
```

THE END !
