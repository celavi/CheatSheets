# Vagrant Cheat Sheet
## Basics
### creat vagrant box from precise64
```bash
mkdir some_folder
cd some_folder
vagrant init precise64 http://files.vagrantup.com/precise64.box
```
### run box
```bash
vagrant up
```
### check status
```bash
vagrant status
```
### SSH to box
```bash
vagrant ssh
```
### suspend box
```bash
vagrant suspend
```
### stop box
```bash
vagrant halt
```
### totaly destroy box
```bash
vagrant destroy
```
### copy ssh key to vagrant box
```bash
# determine the port
vagrant ssh-config
# copy ssh key
ssh-copy-id vagrant@127.0.0.1 -p <port_number>
```
## Plugins
### list all plugins
```bash
vagrant plugin list
```
### install plugin
```bash
vagrant plugin install <plugin-name>
# it works also locally
vagrant plugin install <plugin-gemfile>.gem
```
## Creating a new base box from current one
```bash
# First do some cleaning
sudo rm -rf /var/lib/apt/lists/*
sudo rm -rf /tmp/*
sudo rm -rf /var/tmp/*
# Then remove APT cache
sudo apt-get clean
# Then, “zero out” the drive (for Ubuntu, Debian):
sudo dd if=/dev/zero of=/EMPTY bs=1M
sudo rm -f /EMPTY
# clear the Bash History
cat /dev/null > ~/.bash_history && history -c
# clear the zsh history
echo "" > ~/.zsh_history & exec $SHELL -l
#  repackage the server we just created into a new Vagrant Base Box
vagrant package --output brandNewBox.box
# add this new Vagrant Box into Vagrant
vagrant box add namespace/myNewBox brandNewBox.box
```