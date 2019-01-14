# Provisioning Compute Resources

Kubernetes requires a set of machines to host the Kubernetes control plane and the worker nodes where containers are ultimately run. In this lab you will provision the compute resources required for running a secure and highly available Kubernetes cluster across a single [compute zone](https://cloud.google.com/compute/docs/regions-zones/regions-zones).

> Ensure a default compute zone and region have been set as described in the [Prerequisites](01-prerequisites.md#set-a-default-compute-region-and-zone) lab.

## Networking

The Kubernetes [networking model](https://kubernetes.io/docs/concepts/cluster-administration/networking/#kubernetes-model) assumes a flat network in which containers and nodes can communicate with each other. In cases where this is not desired [network policies](https://kubernetes.io/docs/concepts/services-networking/network-policies/) can limit how groups of containers are allowed to communicate with each other and external network endpoints.

> Setting up network policies is out of scope for this tutorial.

### Import an Existing SSH Key

If you don't already have an ssh key, create one using GitHub's [documentation](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)

```
doctl compute ssh-key import new_key_name \
  --public-key ~/.ssh/id_rsa.pub
```

If you've already created an ssh key, get the ssh key's fingerprint. We'll use the fingerprint when we create our droplets.

```
doctl compute ssh-key list
```

## Compute Instances

The compute instances in this lab will be provisioned using [Ubuntu Server](https://www.ubuntu.com/server) 18.04, which has good support for the [containerd container runtime](https://github.com/containerd/containerd). Each compute instance will be provisioned with a fixed private IP address to simplify the Kubernetes bootstrapping process.

### Kubernetes Controllers

Create three compute instances which will host the Kubernetes control plane:

```
for i in 0 1 2; do
  doctl compute droplet create controller-${i} \
    --size s-1vcpu-2gb \
    --image ubuntu-18-04-x64 \
    --region sfo2 \
    --enable-private-networking \
    --ssh-keys "ssh:key:fingerprint:0a:0b" \
    --tag-names kubernetes-the-hard-way,controller
done
```

### Kubernetes Workers

Create three compute instances which will host the Kubernetes worker nodes:

```
for i in 0 1 2; do
  doctl compute droplet create worker-${i} \
    --size s-1vcpu-2gb \
    --image ubuntu-18-04-x64 \
    --region sfo2 \
    --enable-private-networking \
    --ssh-keys "ssh:key:fingerprint:0a:0b" \
    --tag-names kubernetes-the-hard-way,worker
done
```

### Verification

List the compute instances in your default compute zone:

```
doctl compute droplet list
```

> output

```
ID           Name            Public IPv4        Private IPv4      Public IPv6    Memory    VCPUs    Disk    Region    Image               Status    Tags                                  Features              Volumes
128594669    controller-0    XXX.XXX.XXX.XXX    10.138.154.27                    2048      1        50      sfo2      Ubuntu 18.04 x64    active    kubernetes-the-hard-way,controller    private_networking
128594673    controller-1    XXX.XXX.XXX.XX     10.138.154.135                   2048      1        50      sfo2      Ubuntu 18.04 x64    active    kubernetes-the-hard-way,controller    private_networking
128594681    controller-2    XXX.XXX.XXX.XX     10.138.154.173                   2048      1        50      sfo2      Ubuntu 18.04 x64    active    kubernetes-the-hard-way,controller    private_networking
128594688    worker-0        XXX.XXX.XXX.XX     10.138.154.156                   2048      1        50      sfo2      Ubuntu 18.04 x64    active    kubernetes-the-hard-way,worker        private_networking
128594689    worker-1        XXX.XXX.XXX.XX     10.138.90.73                     2048      1        50      sfo2      Ubuntu 18.04 x64    active    kubernetes-the-hard-way,worker        private_networking
128594691    worker-2        XXX.XXX.XXX.XX     10.138.90.77                     2048      1        50      sfo2      Ubuntu 18.04 x64    active    kubernetes-the-hard-way,worker        private_networking
```

## Configuring SSH Access

SSH will be used to configure the controller and worker instances. When connecting to compute instances for the first time SSH keys will be generated for you and stored in the project or instance metadata as describe in the [connecting to instances](https://cloud.google.com/compute/docs/instances/connecting-to-instance) documentation.

Test SSH access to the `controller-0` compute instances:

```
doctl compute ssh controller-0
```

You'll be logged into the `controller-0` instance:

```
Welcome to Ubuntu 18.04.1 LTS (GNU/Linux 4.15.0-42-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

...

root@controller-0:~#
```

Type `exit` at the prompt to exit the `controller-0` compute instance:

```
root@controller-0:~$ exit
```
> output

```
logout
Connection to XX.XXX.XXX.XXX closed
```

Next: [Provisioning a CA and Generating TLS Certificates](04-certificate-authority.md)
