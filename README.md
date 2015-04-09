# coreos-vagrant-environment
Coreos Vagrant Environment


This project allows you to spin up a coreos host with docker, etcd and fleet all exposed via tcp. 
Folow these steps to get up and running:

1. Copy config.rb.sample to config.rb - this file contain ports that can be changed to your liking. The default setup should work though.
2. Copy user-data.sample to user-data - this is a cloud config that tells fleet which units to start.
3. If you are using a private docker registry, run docker login on your local machine and copy the contents of ~/.dockercfg to the 'Write Files' section of user-data.
4. Vagrant up

Have fun with CoreOS!
