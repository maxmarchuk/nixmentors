<!---
   Copyright 2014 Portland State University

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
--->

Lab 3: NFS/LDAP/DNS
===================


In this lab you will be playing with NFS, the Network File System; LDAP, the Leightweight Directory Access Protocol, and DNS; the Domain Name Service. These are kindof the hardcore services. For Realz. Dis aint the Web Dump, homie.


Section 1: Networking Basics
----------------------------


For this lab we will have a multi-vm setup inside vagrant. All three vms will be running Centos Linux. Each will have its own IP Address.



Take a quick look at your vagrantfile:

This file can be your reference for which machines have which names and ip addresses.


### Vagrant up

```shell
vagrant up
```

```shell
hadron:Lab3-NFS-LDAP-DNS (master) $ vagrnt status
zsh: correct 'vagrnt' to 'vagrant' [nyae]? y
Current machine states:

nfsserver                 running (virtualbox)
nfsclient1                running (virtualbox)
nfsclient2                running (virtualbox)
nfsclient3                running (virtualbox)

This environment represents multiple VMs. The VMs are all listed
above with their current state. For more information about a specific
VM, run `vagrant status NAME`.
```

### SSH into multiple hosts

Open up several terminal windows, change directory on all of them into the Lab3 directory.

```shell

vagrant ssh nfs_server
```

And in a different window:


```shell
vagrant ssh nfs_client_1
```

### Exercises

#### Basic networking verification and warm up

* Ping each host from one host

```shell
hadron:Lab3-NFS-LDAP-DNS (master) $ vagrant ssh nfsserver
Last login: Tue Apr 30 21:45:04 2013 from 10.0.2.2
Welcome to your Vagrant-built virtual machine.
vagrant@nfsserver: ~ > ping nfsclient1
ping: unknown host nfsclient1
vagrant@nfsserver: ~ > ping 192.168.1.11
PING 192.168.1.11 (192.168.1.11) 56(84) bytes of data.
64 bytes from 192.168.1.11: icmp_seq=1 ttl=64 time=0.283 ms
64 bytes from 192.168.1.11: icmp_seq=2 ttl=64 time=0.268 ms
^C
--- 192.168.1.11 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1611ms
rtt min/avg/max/mdev = 0.268/0.275/0.283/0.018 ms
```

* View the routing table for your virtual machines

```
vagrant@nfsserver: ~ > netstat -rn
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
192.168.1.0     0.0.0.0         255.255.255.0   U         0 0          0 eth1
10.0.2.0        0.0.0.0         255.255.255.0   U         0 0          0 eth0
169.254.0.0     0.0.0.0         255.255.0.0     U         0 0          0 eth0
169.254.0.0     0.0.0.0         255.255.0.0     U         0 0          0 eth1
0.0.0.0         10.0.2.2        0.0.0.0         UG        0 0          0 eth0
````


* Set up three users on nfs_server and nfs_client_1
        * ashley: uid=2313
        * bob:    uid=2121
        * mike:   uid of your choice
* Set passwords for these users
* Create a file foo.txt with plain text in it, and copy it using scp from one host to another using your created users
* Set up a hosts file on nfs_server to map each ip address in your network to the common name given in the vagrant file.
* Copy that hosts file to all other servers in your infrastructure




Section 2: NFS: The Network File System
---------------------------------------

NFS, above all other *nix services, is the core of what the CAT does. Since the dawn of multisystem networking janaka and his minions have been carving space off of the big iron and exporting it to the puny clients.

On a conceptual level, NFS is one computer letting another computer use its storage space. It is also one-to-many, meaning that one NFS server can export a filesystem and many client computers can mount and utilize this file system.


### Exercises

* Install NFS server and client from packages
* Start these daemons
* Create directories /data/share1 through /data/share7

### /etc/exports

The NFS server is configured by a file called /etc/exports. This file tells the nfs server which directories to export and which machines to export them to. NFS, on its own, does not do per-user authentication. Rather any machine in the access list can mount the remote filesystem with the options given in /etc/exports.

You can read about /etc/exports at http://www.centos.org/docs/5/html/Deployment_Guide-en-US/s1-nfs-server-config-exports.html

### Exercises

* Set the directory /data/share1 to be shared with standard permissions to all hosts
* Execute ``exportfs -rv`` on nfs_server
* Mount the filesystem on nfs_client_1

```shell

root@nfs_client_1 ~# mkdir /data
root@nfs_client_1 ~# mkdir /data/share1
root@nfs_client_1 ~# mount -t nfs nfs_server:/data/share1 /data/share1


```

### Exercises (All of these should be tested)

* Share /data/share2 to only nfs_client_2
* Share /data/share3 to only clients in the 192.168.1.0/24 subnets
* Share /data/share4 to all clients in your subnet but no other ip addresses, but turn off root squashing

### Exercises 

* On the clients, verify that you can create a file on an NFS share with one user, and pull it off with another user on another system
* On one client, create a large file and open it with vim. On another client, rm the file. What happens?




Section 3: LDAP Client connection
---------------------------------------

Section 4: DNS
---------------------------------------



