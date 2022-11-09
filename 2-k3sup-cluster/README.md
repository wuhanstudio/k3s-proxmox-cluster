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

# Test your cluster with:
$ export KUBECONFIG=/home/wuhanstudio/kubeconfig
$ kubectl config use-context default
$ kubectl get node -o wide
```

**Congratulations! The k3s cluster is ready for deployment.**

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
