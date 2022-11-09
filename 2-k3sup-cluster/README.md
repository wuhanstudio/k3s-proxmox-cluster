## K3S Cluster

We first install a tool, [k3sup](https://github.com/alexellis/k3sup).

```
# On your Local PC
$ curl -sLS https://get.k3sup.dev | sh
$ sudo install k3sup /usr/local/bin/
```

Now we are ready to set up the k3s cluster.

```
# Make sure you can ssh into each machine without password

# k3s master
# You may consider increasing the memory of the master node to be 2GB 
$ k3sup install --ip 192.168.1.130 --user ubuntu

# k3s worker1
$ k3sup join --ip 192.168.1.114 --server-ip 192.168.1.130 --user ubuntu

# k3s worker2
$ k3sup join --ip 192.168.1.115 --server-ip 192.168.1.130 --user ubuntu
```

**Congratulations! The k3s cluster is ready for deployment.**

```
# Test your cluster with:
$ export KUBECONFIG=/home/wuhanstudio/kubeconfig
$ kubectl config use-context default
$ kubectl get node -o wide
```

Outputs:

```
NAME      STATUS   ROLES                  AGE   VERSION
master    Ready    control-plane,master   53m   v1.25.3+k3s1
worker1   Ready    <none>                 50m   v1.25.3+k3s1
worker2   Ready    <none>                 46m   v1.25.3+k3s1
```
--------

### Optional (Swap Area)

```
## Swap Area

$ sudo fallocate -l 1G /swapfile
$ sudo mkswap /swapfile
$ sudo swapon /swapfile
$ echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
$ sudo sysctl vm.swappiness=10
$ echo 'vm.swappiness=10' | sudo tee -a /etc/sysctl.conf
```

### Clean up (if anything goes wrong)

To uninstall K3s from a server node, run:

```
/usr/local/bin/k3s-uninstall.sh
```

To uninstall K3s from an agent node, run:

```
/usr/local/bin/k3s-agent-uninstall.sh
```
