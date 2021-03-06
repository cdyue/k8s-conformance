# Conformance tests for WiseCloud v1.5.1 Kubernetes

## Install Diamanti D10 v2.1.0 (base on Kubernetes v1.12.1) and create a cluster


Diamanti D10 cluster:

[root@appserv9 diamanti]# dctl cluster status
Name           	: devtb6
UUID           	: 2bc2854f-e2bd-11e8-9821-2c600c82ec72
State          	: Created
Version        	: 2.1.0 (0)
Master         	: appserv9
Etcd State     	: Healthy
Virtual IP     	: 172.16.19.23
Storage VLAN   	: 430
Pod DNS Domain	: cluster.local

NAME                      NODE-STATUS   K8S-STATUS   MILLICORES   MEMORY          STORAGE    IOPS      VNICS     BANDWIDTH   SCTRLS          LABELS
                                                                                                                             LOCAL, REMOTE
appserv10/(etcd)          Good          Good         0/32000      1GiB/64GiB      0/3.05TB   0/500K    2/63      0/40G       0/64, 0/64      beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/hostname=appserv10
appserv11/(etcd)          Good          Good         0/32000      1GiB/64GiB      0/3.05TB   0/500K    2/63      0/40G       0/64, 0/64      beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/hostname=appserv11
appserv9/(master, etcd)   Good          Good         250/32000    1.06GiB/64GiB   0/3.05TB   0/500K    2/63      0/40G       0/64, 0/64      beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/hostname=appserv9




## Run Conformance Test

The standard tool for running these tests is
[Sonobuoy](https://github.com/heptio/sonobuoy).  Sonobuoy is
regularly built and kept up to date to execute against all
currently supported versions of kubernetes, and can be obtained [here](https://github.com/heptio/sonobuoy/releases).

Download the CLI by running:

```
$ go get -u -v github.com/heptio/sonobuoy
```

Deploy a Sonobuoy pod to your cluster with:

```
$ sonobuoy run
```

View actively running pods:

```
$ sonobuoy status
```


To inspect the logs:

```
$ sonobuoy logs
```

Once `sonobuoy status` shows the run as `completed`, copy the output directory from the main Sonobuoy pod to
a local directory:

```
$ sonobuoy retrieve .
```

This copies a single `.tar.gz` snapshot from the Sonobuoy pod into your local
`.` directory. Extract the contents into `./results` with:

```
mkdir ./results; tar xzf *.tar.gz -C ./results
```

**NOTE:** The two files required for submission are located in the tarball under **plugins/e2e/results/{e2e.log,junit.xml}**.

To clean up Kubernetes objects created by Sonobuoy, run:

```
sonobuoy delete
```

