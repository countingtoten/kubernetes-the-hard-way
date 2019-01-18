# Cleaning Up

In this lab you will delete the compute resources created during this tutorial.

## Compute Instances

Delete the controller and worker compute instances:

```
doctl compute droplet delete \
  controller-0 controller-1 controller-2 \
  worker-0 worker-1 worker-2
```

## Networking

Delete the external load balancer network resources:

```
{
  doctl compute load-balancer delete $(doctl compute load-balancer list | grep controller | awk '{ print $1 }')
}
```
