
Install dependencies

```bash
#  yum install docker atomic kubernetes etcd
```
Configure docker storage pool

```bash
#  nano  /etc/sysconfig/docker-storage-setup
```
Paste these two lines inside

```bash
DEVS=/dev/vdb
VG=docker-vg
```
Press ctr+o to save and ctr+x to exit

Start and enable Docker

```bash
# systemctl start docker
# systemctl enable docker
```
Configure service account key for Kubernetes

```bash
# mkdir /etc/pki/kube-apiserver
# openssl genrsa -out /etc/pki/kube-apiserver/serviceaccount.key 2048
# sed -i.back '/KUBE_API_ARGS=*/c\KUBE_API_ARGS="--service_account_key_file=/etc/pki/kube-apiserver/serviceaccount.key"' /etc/kubernetes/apiserver
# sed -i.back '/KUBE_CONTROLLER_MANAGER_ARGS=*/c\KUBE_CONTROLLER_MANAGER_ARGS="--service_account_private_key_file=/etc/pki/kube-apiserver/serviceacc
```

Start and enable Kubernetes

```bash
# for SERVICE in etcd kube-apiserver kube-controller-manager  kube-scheduler docker kube-proxy  kubelet; do 
    systemctl restart $SERVICE
    systemctl enable $SERVICE
    systemctl status $SERVICE
done
```
Running an atomicapp

```bash
# atomic run projectatomic/helloapache
```

Have fun.













