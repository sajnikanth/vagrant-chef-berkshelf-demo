This demo shows you how to use chef to provision a vagrant VM. We then see how Berkshelf makes life easier.

Pre-Requisites
==============
* [VirtualBox](https://www.virtualbox.org)
* [vagrant](http://vagrantup.com) installed
* Vagrant omnibus - `vagrant plugin install vagrant-omnibus`
* knife - `gem install knife`

Demo Steps
==========
I've tried to keep commit logs in sync with the actual demo:

* Initialise a vagrant environment using `vagrant init`
* Download `nginx` cookbook to `./cookbooks` using `knife cookbook site download nginx`. The file is unarchived and original is deleted
* Update `Vagrantfile`
    * Forward port 80 to 8080
    * Install chef client via omnibus - `config.omnibus.chef_version = 'latest'`
    * Uncomment chef provisioning
        * Update cookbook path
        * Include `nginx` recipe
* Bring up VM - `vagrant up`
* Observe that provisioning fails because `apt` cookbook was not included
* Observe that provisioning still fails because `build essential`, `ohai`, `runit` and `yum` cookbooks were not included
* Add all cookbooks and provision again - `vagrant provision`

Enter [Berkshelf](http://berkshelf.com/). Now install the following:

        gem install berkshelf && \
        vagrant plugin install vagrant-berkshelf

* create `Berksfile` and add `nginx` cookbook to it. Note that the right way is to use `berks init` but here we are trying to understand the basics with just bare-bones
* update `Vagrantfile` to use berkshelf - `config.berkshelf.enabled = true`
* remove `./cookbooks` folder
* Do a `berks install` and then `vagrant reload`
