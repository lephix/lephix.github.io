# K8s Development Environment

Install a mini K8s env for development.

## K3s

### On Mac with ARM Chip

1. Install `multipass` for getting ready a Linux env.

`brew install --cask multipass`

2. Setup a Ubuntu instance.
`multipass launch --name k3s --mem 4G --disk 40G`

3. Show info of the instance.
`multpass info k3s`

4. We can mount a local path into the VM, for sharing convenience.
`multipass mount ~/test/k8s k3s:~/k8s`

5. Install K3s on the VM.
```bash
multipass shell k3s
curl -sfL https://get.k3s.io | sh -
```

6. By default, `/etc/rancher/k3s/k3s.yaml` is the K8s config file.

7. Add another node to the K3s cluster. 

Get info from the current cluster.
```bash
multipass exec k3s sudo cat /var/lib/rancher/k3s/server/node-token
multipass info k3s | grep -i ip
```

Create a node and add to the current cluster.
```bash
$ multipass launch --name k3s-worker --mem 2G --disk 20G
$ multipass shell k3s-worker
ubuntu@k3s-worker:~$ curl -sfL https://get.k3s.io | K3S_URL=https://192.168.64.4:6443 K3S_TOKEN="hs48af...947fh4::server:3tfkwjd...4jed73" sh -
```

Verify the cluster.
```bash
kubectl get nodes
```

8. Remove Node
```bash
multipass delete k3s k3s-worker
multipass purge
```