# saltstack vagrant lab
A vagrant lab for working with the saltstack.

Run the following commands in a terminal. Git, VirtualBox and Vagrant must already be installed.

```
git clone https://github.com/Ali-Yazdani/saltstack_vagrant_lab.git
cd saltstack_vagrant_lab
vagrant plugin install vagrant-vbguest
vagrant up
```

This will download Ubuntu and CentOS VirtualBox images and create three virtual machines for you. One will be a Salt Master named master and two will be Salt Minions named minion1 (run on Ubuntu) and minion2 (run on CentOS). 

If all done fine, you can run the following commands for first steps working with the salt lab:
```
vagrant ssh master

# After conneting 
sudo -s
salt '*' test.ping
```


*P.S: Heavily inspired by https://github.com/UtahDave/salt-vagrant-demo.*
