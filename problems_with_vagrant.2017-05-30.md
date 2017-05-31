# Problem with Vagrant + Virtualbox

2017-05-30

Computer froze while Vagrant was booting up. The machine index was corrupted from the freeze, and my vagrant box was not accessible. Went through several troubleshooting steps to resolve the issue, and eventually got my machine back up.

## Steps + Errors

0. Error:
```
The machine index which stores all required information about
running Vagrant environments has become corrupt. This is usually
caused by external tampering of the Vagrant data folder.

Vagrant cannot manage any Vagrant environments if the index is
corrupt. Please attempt to manually correct it. If you are unable
to manually correct it, then remove the data file at the path below.
This will leave all existing Vagrant environments "orphaned" and
they'll have to be destroyed manually.

Path: C:/Users/Username/.vagrant.d/data/machine-index/index
```

* [Corrupted index file, Stack overflow](https://stackoverflow.com/questions/24911021/vagrant-corrupted-index-file-c-users-username-vagrant-d-data-machine-index-ind)

1. Navigated to ~/.vagrant.d/data/machine-index/
ran: ``rm -f *``

2. Hit this error:
```
The VirtualBox VM was created with a user that doesn't match the current user running Vagrant. VirtualBox requires that the same user be used to manage the VM that was created. Please re-run Vagrant with that user. This is not a Vagrant issue.

The UID used to create the VM was:
Your UID is: 1000
```

* [Stackoverflow: UID doesn't match current user](https://stackoverflow.com/questions/31644222/vagrant-not-starting-up-user-that-created-vm-doesnt-match-current-user )

Navigated to ``~/Documents/.vagrant/machines/default/virtualbox/``
ran: ``echo -n 1000 >> creator_uid && vagrant up``

4. Vagrant brought up a new machine, not my original VM. Had to get my old box back.
[vagrant box disconnected](https://github.com/mitchellh/vagrant/issues/1755)

Navigated to: ``~/Virtualbox\ VMs/Documents_default_1492630824800_98335/Documents_default_1492630824800_98335.vbox-prev``

found this line:
```
  <Machine uuid="{db179e00-3ba1-4e08-a86a-ad7ab69ddac4}" name="Documents_default_1496197504242_4039" OSType="Ubuntu_64" snapshotFolder="Snapshots" lastStateChange="2017-04-18T22:10:38Z">
```

5. Navigate to
``/home/concati/Documents/.vagrant/machines/default/virtualbox``

ran this: 

```
echo -n db179e00-3ba1-4e08-a86a-ad7ab69ddac4 > id
vagrant up
```
6. Got my development box back.
7. Destroyed all other machines that I didn't need through the virtualbox GUI.


## Auxillary problems
Problem: ``vagrant ssh`` was not authenticating correctly.

1. Generated a new ssh key: ``ssh-keygen -t rsa -b 4096 -C "vagrant"``
2. Moved the new key to ``/home/$(whoami)/Documents/.vagrant/machines/default/virtualbox/private_key``
3. Booted into the vagrant box and copied the contents of id_rsa.pub into ``~/.ssh/authorized_keys``
4. Deleted the old public key in that file.
