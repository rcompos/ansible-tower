# zero

Development environment using Vagrant and Virtualbox.

The control node zero runs CentOS 7 and includes a GUI.  It is designated as a jump host for the other VM's in the virtual private network.

```
 ____________________________________
<  The only good bug is a dead bug.  >
 ------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)        )\/\
                ||----w |
                ||     ||
```
## Requirements

This development environment requires the following on the host machine (i.e. your MacBook or PC):

  - Virtualbox (VMware Fusion or VMware Workstation are supported with plugin)
  - Vagrant
  - Git

#### Clone repo
Clone this repository to working directory on local host and change to directory.

`$ git clone https://github.com/rcompos/zero`

`$ cd zero`

#### Install Vagrant

The Vagrant recommendation is to download installer from https://www.vagrantup.com/downloads.html

*MacOS*: `$ brew cask install vagrant`

#### Install Vagrant Plugin(s)

Install Vagrant plugins using native plugin manager.  
VMware Fusion and Desktop are supported with a paid plugin.

#### Create VM Definitions (optional)

Virtual machine definitions are stored in the file *Vagrantfile*.  Customize as needed.
A large number of VM definitions which adds some notable lag to Vagrant start-up times.
Start-up lag can be remedied by pruning unwanted definitions.

If you want to address the guest VM's by host name, you must create DNS entries or entries
on the host VM.

## Setup
#### Spin up Control VM

Change to *zero* directory and run the following as your user (root not required).

Get status.

`$ vagrant status`

Start set of VM's.

`$ vagrant up zero`

Login to control server and change to root. 

`$ vagrant ssh zero`

On server *zero* change to root and run ansible ping module to connect to all nodes.
*Execute as root on server 'zero'.*  Type yes and press return.

```
zero$ sudo su -
zero$ ansible all -m ping   #  * type yes then enter *
```

#### Spin up Client VM's

Change to *zero* directory and run the following as your user (root not required).

Get status.

`$ vagrant status`

Start set of VM's.

`$ vagrant up zero cent7s0`

Bounce servers to disable SELinux (unfortunate for now).

`$ vagrant reload zero cent7s0`


#### Update system hosts and ansible hosts

*** _Run this each time a VM is created or destroyed_ ***

The hosts info must be updated and ssh keys configured.  Change to *zero* directory and run the following as your user (root not required).

Update hosts files.

`$ vagrant hostmanager`

Re-run provision scripts to update Ansible hosts.

`$ vagrant provision zero`


## Develop

Log on to the zero server.  Update your git identity.

```
root@zero ~ $ git config --global user.name "John Doe"
root@zero ~ $ git config --global user.email johndoe@example.com
```

Change to your desired location and clone some repo for development.
The */src* directory on the control VM is shared from the host's filesystem at *../src* (directory named *src* in parent directory).

```
root@zero ~ $ cd /src
root@zero /src $ git clone https://github.com/<user>/<repo>.git
```

Develop and test using server zero as control node.  Enjoy git branch status in prompt.
All files in *src* are stored outside the zero repo.
The *src* directory will persist on host filesystem even if VM's are destroyed or the zero repo is removed.

```
root@zero /sr $ cd my-repo
root@zero /src/my-repo (mybranch *) $
```

## Destruction

#### Stop VM's

Stop VM's.

`$ vagrant halt`

#### Destroy VM's to abandon or rebuild

`$ vagrant destroy -f zero cent7s0`

## Helpful Commands

Start all VM's.

`$ vagrant up`

Start single VM.

`$ vagrant up zero`

Start multiple VM's.

`$ vagrant up dbserver webserver`

Stop single VM.

`$ vagrant halt dbserver`

Stop multiple VM's.

`$ vagrant halt dbserver webserver`

Get global status.

`$ vagrant global-status`
