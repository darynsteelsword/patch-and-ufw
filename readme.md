Tech Test for TP Readme
=================

This is the readme for using the Ansible code in this repo, the design of said code enabling the patching, installation, configuration/enabling of UFW and rebooting of a remote Ubuntu box.

requirements
------------

#### On the local system
* On the local system which will initialise the configuration of the remote Ubuntu system:
  * Ensure git and ansible are installed.
  * As root, run the commands:

```
useradd -m ansuser
su ansuser -
cd ~ && mkdir .ssh && chmod 700 .ssh
ssh-keygen -f ~/.ssh/id_rsa -t rsa -N ''
cat ~/.ssh/id_rsa.pub 		# copy the contents of this file, ready for the next steps
```

#### On the remote system
* On the remote system, ie the Ubuntu box to be configured:
  * As root, run the commands:

```
apt-get install openssh-server && service sshd status
useradd -m ansuser
su ansuser -
cd ~ && mkdir .ssh && chmod 700 .ssh
vi ~/.ssh/authorized_keys		# copy in the contents of id_rsa.pub from the local system
                                        # could've used ssh-copy for this but allowing root ssh on a box is unpleasant
```

#### On the local system
* On the local system:
  * In /opt, as root, run the commands:

```
git clone https://github.com/techtesttp/patch-and-ufw.git
chown -R ansuser:ansuser patch-and-ufw
su ansuser -
cd /opt/patch-and-ufw/
nano inventory/ubuntuservers # (use editor of choice here, vi, nano etc)
```

* In the inventory/ubuntuservers file, replace 192.168.0.17 with the IP address or DNS name of the Ubuntu server to be patched and configured, then save and close.
* Finally, still as the "ansuser" user, run the commands to:
  * initiate the configuring of the remote Ubuntu server
  * install (if not installed already) and configure the UFW (Uncomplicated FireWall)
  * reboot the server and display the uptime when it returns

```
ansible-playbook plays/setup/base.yml -i inventory/ubuntuservers -l ubuntuservers
ansible-playbook plays/ufw/base.yml -i inventory/ubuntuservers -l ubuntuservers
ansible-playbook plays/reboot/base.yml -i inventory/ubuntuservers -l ubuntuservers
```

* The remote system should now be configured (Note, the SSH key generated above for "ansuser" was generated with no password, which is advisable only for service-oriented users, so ensure the private key is kept in a secure location).
