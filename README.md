# trump-vagrant     
*"Make Vagrant great again"* :us:     
***
##### Simple steps to make your vagrant great again
  -  [Increase memory and cpus](#memory-and-cpus)
  -  [Use NFS and private network](#use-nfs-with-private_network)
  -  [*No guest IP was given to the Vagrant core NFS helper*](#no-guest-ip-was-given-to-the-vagrant-core-nfs-helper) (Resolved)


##### Memory and cpus:
First things first, open your **Vagrantfile** and double (if you can) your **v.memory** and **v.cpus**

```ruby
Vagrant.configure("2") do |config|
#...
    config.vm.provider "virtualbox" do |v|
      v.memory = 4096
      v.cpus = 2
    end
end
```
NOTE: For some peoples increase the number of cpus actually slower more his vagrant, that's not my case. Give it a chance.
##### Use NFS with private_network:
Why ? Read this [How to make Vagrant performance not suck](https://stefanwrobel.com/how-to-make-vagrant-performance-not-suck)    
1- Enable **private_network** ( https://www.vagrantup.com/docs/networking/private_network.html )    
The easiest way to use a private network is to allow the IP to be assigned via DHCP if this doesn't work for you use static IP.
```ruby
Vagrant.configure("2") do |config|
  config.vm.network "private_network", type: "dhcp"
end
```
You can also specify a static IP address for the machine. This lets you access the Vagrant managed machine using a static, known IP. The Vagrantfile for a static IP looks like this:  
```ruby
Vagrant.configure("2") do |config|
  config.vm.network "private_network", ip: "192.168.50.4"
end
```
2- Enable **NFS** in **shared folders** ( https://www.vagrantup.com/docs/synced-folders/nfs.html )
```ruby
Vagrant.configure("2") do |config|
  # ...

  config.vm.synced_folder ".", "/vagrant", type: "nfs"
end
```
##### No guest IP was given to the Vagrant core NFS helper:
I highly recommend you to use a Static IP and move on, but if you need to use DHCP to get private network working keep reading...  

Sometimes your vagrant can get different reports about installed GuestAdditions version.     
Check this thread to fix that: https://github.com/chef/bento/issues/232 or skip that and install :  
```shell
$ vagrant plugin install vagrant-vbguest
```
in your vagrant folder.  
More info about *vagrant-vbguest* at https://github.com/dotless-de/vagrant-vbguest   

*Tested in a problematic Vagrant v1.8.1*
