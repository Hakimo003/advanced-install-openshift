
# advanced-install-openshift
It's a Vagrant project to an advanced installation of OpenShift Origin with Ansible

<u>*Prerequisites:*</u>

Install Vagrant & VirtualBox
Install des plugins Vagrant vagrant-hostmanager & landrush


<u>Run</u>

$ vagrant landrush start </br>
$ vagrant up

( .... wait about 10 minutes) and 5 VMs will provisioned: master, node infra, node app, admin, nfs

Configure NFS server:

$ vagrant ssh nfs</br>
$ service nfs start</br>
$ service rpcbind start</br>
$ mkdir -p /exports/volumes/pv{1..10} && mkdir -p /exports/volumes/registry </br>
$ chown nfsnobody:nfsnobody /exports/volumes </br>
$ chown nfsnobody:nfsnobody /exports/volumes/pv{1..10} </br>
  && chown nfsnobody:nfsnobody /exports/volumes/registry </br>
$ vim /etc/exports.d/openshift-WESCALE.exports </br>

/exports/volumes/registry  *(rw,root_squash,no_wdelay) </br>
/exports/volumes/pv1  *(rw,root_squash,no_wdelay) </br>
/exports/volumes/pv2  *(rw,root_squash,no_wdelay) </br>
/exports/volumes/pv4  *(rw,root_squash,no_wdelay) </br>
/exports/volumes/pv3  *(rw,root_squash,no_wdelay) </br>
/exports/volumes/pv5  *(rw,root_squash,no_wdelay) </br>
/exports/volumes/pv6 *(rw,root_squash,no_wdelay) </br>
/exports/volumes/pv7 *(rw,root_squash,no_wdelay) </br>
/exports/volumes/pv8 *(rw,root_squash,no_wdelay)</br>
/exports/volumes/pv9 *(rw,root_squash,no_wdelay)</br>

$ exportfs -rv

In admin vm, you launch the install:

$ vagrant ssh admin </br>
$ su - (password Redhat) </br>
$ cd /home/vagrant && git clone https://github.com/openshift/openshift-ansible </br>
$ cd /home/vagrant/openshift-ansible && git checkout origin/release-3.7 </br>
$ cd /home/vagrant/ && ./deploy.sh (password Redhat)


<u>*Verify the install:*</u>


In master vm:

$ oc login -u system:admin --config=/etc/origin/master/admin.kubeconfig </br>
$ oc get nodes </br>

<u>*Add another cluster admin*</u>

$ htpasswd -b /etc/origin/.htpasswd wescale wescale </br>
$ oadm policy add-cluster-role-to-user cluster-admin Wescale



http://blog.wescale.fr/2018/01/21/simuler-une-installation-dun-cluster-openshift-prod-ready-en-30-minutes/


