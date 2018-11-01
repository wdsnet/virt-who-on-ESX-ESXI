# Configuring virt-who-on-ESX/ESXI
-------------
## Requirements 
   - Subscription: Red Hat Enterprise Linux  for Virtual Datacenters  activated ;
   - At least one ESX/ESXI host; 
   - One vCenter;
   - At least one Guest with Linux installed or more.
   -------------
## Introduction

 The **VDC (Virtual Datacenters) subscription** offers the way to run an unlimited amount of RHEL VMs on a hypervisor supported.
 
 **Hypervisors:**
   - Xen 
   - Hyper-v 
   - RHEV-M 
   - ESX 
   - Kubevirt
   -------------
The **virt-who** is the tool that helps in managing the VDC licenses allowing the subscribers to subscribe.

#### 1. Create vCenter user (suggest virt-who@vsphere.local) and password
#### 2. Set user roles read-only from vCenter
#### 3. Register RHEL VM for install package from RHEL repo.
```console
$ subscription-manager register --user=[RHSM_USERNAME] --auto-atach
```
#### 4. Install **virt-who** package 
```console
$ yum install virt-who -y 
```
#### 5. Get **ORG Id** (Owner) 
```console
$ subscription-manager orgs --user=[RHSM_USERNAME]
```

 **10XXXXXXXX** 

#### 6. Unregister RHEL VM
```console
$ subscription-manager unregister --user=[RHSM_USERNAME] 
$ subscription-manager clean 
```
#### 7. Encrypt passwords vCenter and RHSM from **virt-who-password** tool:
```console
$ virt-who-password  [Type vCenter password]
377ead025e077be34b7b620e2e421b4
```
```console 
$virt-who-password  [Type RHSM password] 
c4565893a2dfc57dbb73928447d568d8
```
#### 8. Create /etc/virt-who.d/virt-who.conf file with content bellow:
```vim
[vmware]
type=esx
server=vCenter IP or hostname]
username=virt-who@vsphere.local 
encrypted_password=377ead025e077be34b7b620e2e421b4
owner=10XXXXXXXX                                                
rhsm_username=[RHSM username]                                
rhsm_encrypted_password=c4565893a2dfc57dbb73928447d568d8   
env=Library
hypervisor_id=hostname
```
#### 9. Start and enable it.
```console
$ systemctl  start virt-who 
$ systemctl  enable virt-who 
```
#### 10. Check /var/log/rhsm/rhsm.log log file.
```console
$ systemctl  enable virt-who `
```

