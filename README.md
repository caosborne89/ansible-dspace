# Dspace Ansible

## Requirments
* Ubuntu 16.04 Virtual Machine
* Vagrant and Virtualbox installed on host
* Ansible installed on host machine
* Currently only tested on Ansible 2.4.2.0


## Getting started
* Copy site.yml.EXAMPLE to site.yml
* Change `linux_user_param` to your linux user on the virtual machine (or just 'root')
* Make sure your virtual machine is added to your Ansible inventory. In Ubuntu (host machine) change the `hosts` to what ever host group you decide to put the virtual machine under. For example, if you decided to keep `webservers` as the hosts, your `hosts` file in ansible would look like this:
```
[webservers]
ansible
```
* Run ansible playbook
```
ansible-playbook site.yml --extra-vars "ansible_sudo_pass=YOURPASSOWORD"
```
* You will probably have to run this twice (it fails the first time and then automatically installs what it needs to run apt-get).

* ssh into the virtual machine and run this command from the `/dspace` directory to create and admin account 
```
bin/dspace create-administrator
```

## Important Notes

* To restart, stop, or star tomcat run
```
sudo systemctl (restart | stop | start) tomcat
```

* It would be helpful to use port forwarding for ssh and for the tomcat port. For example in VirtualBox this might look like this:


Name | Protocol | Host IP | Host Port | Guest IP | Guest Port |  
---  | --- | --- | --- | --- | --- | 
SSH | TCP | 127.0.0.1 | 2222 | 10.0.2.15 | 22 |
Web | TCP | 127.0.0.1 | 8081 | 10.0.2.15 | 8080 |

You should be able to ssh to the virtual machine and point to http://localhost:8081 in the web browser on your local machine to view dspace in the browser

## Using Vagrant
* Run <code>$ vagrant up</code> from the install directory (same level as the Vagrantfile).
* After Vagrant has setup (several minutes later), it should be possible to see your installation in the browser.
* The host's 'src' directory is a shared directory with the Dspace source files, so your IDE can edit them.
* To pass build commands to the guest VM please use something like the following command:
```
$ vagrant ssh -c 'cd /data1/dspace-src; sudo mvn package -P !dspace-xmlui-mirage2,!dspace-xmlui,!dspace-rdf,!dspace-rest,!dspace-sword,!dspace-swordv2,!dspace-oai'
```
