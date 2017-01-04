# Creating a Spinnaker applicance with Kubernetes

This guide will cover how I created a bare-metal continuous-delivery applicance using Spinnaker, Kubernetes and some inexpensive "mini pcs".

The first version of this guide can be found here: https://hackernoon.com/diy-kubernetes-cluster-with-x86-stick-pcs-b0b6b879f8a7

## Spinnaker on bare metal

http://www.spinnaker.io/ is set of microservices that make it easy to build continuous delivery pipelines. Most of the time it is run on a cloud platform such as AWS or GCP. While this works great, it can be pretty expensive due to the resource requirements ($200+ a month).

Kubernetes is a first class deploy target with Spinnaker. We are taking this a step further by running SPinnaker on Kubernetes itself. The great thing about Kubernetes is it can run in the cloud and also on bare-metal. By setting up our own cluster, we can in a sense run our own "cloud" on our own hardware. With the advent of cheap "mini pcs" we can have our own target environment that can then deploy to the cloud. 

## Choosing hardware

While I was able to get the cluster turned up with the first version of this guide, The "Stick pcs" proved to be too weak on the memory requirement. I needed nodes that had at least 4gb memory. After some searching I decided on three "nexbox" pcs.

https://www.aliexpress.com/store/product/1Set-Nexbox-T9-Smart-TV-Box-Z8300-1-84GHz-4-Cores-Win-10-Mini-PC-4GB/2130214_32658221265.html

* 4GB memory
* Quad core atom processor
* 64GB SSD

It took a while for the boxes to arrive from Aliexpress but I was excited to get started when they did.

![alt](img1.png)

## Installing Ubuntu

Unlike the tv-sticks, these boxes came with windows installed. That was no good for the cluster so I began by trying to install ubuntu server. Unfortunatley since the chipset in these machines was "cherry trail" it had limited linux compatability. The nic did not work at all during install.
![alt](img2.png)
Thankfully after some searching I was able to find an ubuntu image with a modifed kernal to support the chipset. http://linuxiumcomau.blogspot.com/search?updated-min=2017-01-01T00:00:00%2B11:00&updated-max=2018-01-01T00:00:00%2B11:00&max-results=2

![alt](img3.png)

With ubuntu installed on the boxes I was now able to install docker on each node.

## Installing Kubernetes

```
apt-get update
apt-get install docker.io 
```

I then leveraged the docker-multinode scripts to get kubernetes installed along with heapster and the dashboard.

https://github.com/kubernetes/kube-deploy

Installation is relativly simple, you run the master.sh script on one node and worker.sh on the remaining two nodes.

Tunneling 8080 into the master node and the dashboard displayed like a charm.

![alt](img4.png)

## Installing Spinnaker

I then was able to make some minor modifications to the "spinikube" specs and get spinnaker installed.

https://github.com/kenzanlabs/spinikube

![alt](img5.png)

Overall I am very happy with how the cluster turned out. It's great to be able to have a dedicated cluster without wasting resources on vms. I'm looking forward to running more workloads and monitoring performance.

![alt](img6.png)

Next steps to experiment with include getting persistent volume storage in place with ceph or gluster. It will be great to take advantage of all the storage on the nodes. 