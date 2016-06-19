
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
# systemctl start docker
# systemctl enable docker
