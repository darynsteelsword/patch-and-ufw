Tech Test for TP Readme
=================

This is the readme for using the Ansible code in this repo, said codes design enabling the patching and the enabling of UFW on a remote Ubuntu box.

requirements
------------

* On the local system which will initialise the configuration of the remote Ubuntu system:
* Ensure git and ansible are installed.
    * As root, run the commands:

```
useradd -m ansuser
su ansuser -
cd ~ && mkdir .ssh && chmod 700 .ssh
ssh-keygen -f ~/.ssh/id_rsa -t rsa -N ''
cat ~/.ssh/id_rsa.pub 		# copy the contents of this file, ready for the next steps)
```

* On the remote system, ie the Ubuntu box to be configured:
* As root, run the commands:

```
apt-get install openssh-server && service sshd status
useradd -m ansuser
su ansuser -
cd ~ && mkdir .ssh && chmod 700 .ssh
vi ~/.ssh/authorized_keys		# copy in the contents of id_rsa.pub from the local system
```


* In /opt, as root, run the commands:

```
git clone https://github.com/techtesttp/patch-and-ufw.git
chown -R ansuser:ansuser patch-and-ufw.git
su ansuser -
cd /opt/patch-and-ufw
nano inventory/ubuntuservers # (use editor of choice here)
```

* In the inventory/ubuntuservers file, replace 192.168.0.17 with the IP address or DNS name of the Ubuntu server to be patched and configured, then save and close.
* Finally, still as the "ansuser" user, run the command to initiate the configuring of the remote Ubuntu server:

```
ansible-playbook plays/setup/base.yml -i inventory/ubuntuservers -l ubuntuservers
```

* The remote system should now be configured (Note, the SSH key generated above for "ansuser" was generated with no password, which is advisable only for service-oriented users, so ensure the private key is kept in a secure location).
