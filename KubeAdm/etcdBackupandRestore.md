# etcd Backup and Restore

etcd is a critical component in Kubernetes clusters, storing all cluster data. Regular backups and knowing how to restore from them are crucial for disaster recovery.

## Installing etcd Client

If the etcd client is not installed, you can easily install it using the package manager on Ubuntu or Debian-based systems. Here are the steps:

1. Update the package list:

sudo apt-get update

2. Install the etcd-client package:


sudo apt-get install -y etcd-client

## Backup etcd Data

```bash
sudo ETCDCTL_API=3 etcdctl snapshot save /var/backups/etcd/snapshot-new.db \
--endpoints=https://127.0.0.1:2379 \
--cacert=/etc/kubernetes/pki/etcd/ca.crt \
--cert=/etc/kubernetes/pki/etcd/server.crt \
--key=/etc/kubernetes/pki/etcd/server.key
```

### Verifying the Backup

To verify the backup, use:

```bash
sudo ETCDCTL_API=3 etcdctl snapshot status /var/backups/etcd/snapshot-new.db
```

## Restore etcd Data

To restore the etcd data, follow these steps:

```bash
export ETCDCTL_API=3
etcdctl  snapshot restore  /var/backups/etcd/snapshot-new.db --data-dir /var/lib/etcd-restore \
--endpoints=https://127.0.0.1:2379 \
--cacert=/etc/kubernetes/pki/etcd/ca.crt \
--cert=/etc/kubernetes/pki/etcd/server.crt \
--key=/etc/kubernetes/pki/etcd/server.key
```