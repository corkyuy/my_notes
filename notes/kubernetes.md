# Kubernetes

You will run docker, kubelet, and kube-proxy outside of a container, the same
way you would run any system daemon, so you just need the bare binaries. For
etcd, kube-apiserver, kube-controller-manager, and kube-scheduler, we recommend
that you run these as containers, so you need an image to be built.

* docker
* etcd
    ```
    HostIP=$(ip route get 8.8.8.8 | awk '{print $NF; exit}')
    sudo docker pull quay.io/coreos/discovery.etcd.io
    etcd=$(sudo docker run -d -v /usr/share/ca-certificates/:/etc/ssl/certs -p 4001:4001 -p 2380:2380 -p 2379:2379 \
      --name etcd quay.io/coreos/etcd:v2.3.8 \
      -name etcd0 \
      -advertise-client-urls http://${HostIP}:2379,http://${HostIP}:4001 \
      -listen-client-urls http://0.0.0.0:2379,http://0.0.0.0:4001 \
      -initial-advertise-peer-urls http://${HostIP}:2380 \
      -listen-peer-urls http://0.0.0.0:2380 \
      -initial-cluster-token etcd-cluster-1 \
      -initial-cluster etcd0=http://${HostIP}:2380 \
      -initial-cluster-state new)
    etcd_ip=$(sudo docker inspect --format '{{ .NetworkSettings.IPAddress }}' $etcd)
    ```
* flannel
* easyrsa?
* kubernetes-master
* kubernetes-worker

## ubuntu

```
sudo snap install conjure-up --classic
```




