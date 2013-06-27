## What's Different
Very little has changed from Shawn's original repo, but I've done my best to update things to the current standards of Vagrant and Chef.  The tutorial will be modified to reflect changes, but here are some of the bigger ones.

### New Cookbook management
Rather than submodules to manage 3rd party cookbooks, I've opted to use [Librarian-Chef](https://github.com/applicationsonline/librarian-chef) instead.  **So don't put anything in chef/cookbooks!** That is now taken care of by librarian.  Custom cookbooks go in ```chef/application-cookbooks``` and will need to be added to the Cheffile.

I've also added [Vagrant-Librarian-Chef](https://github.com/jimmycuadra/vagrant-librarian-chef) to automatically fetch and update cookbooks from Vagrant and [Vagrant Omnibus](https://github.com/schisamo/vagrant-omnibus) to keep Chef up to date and working correctly.

### Updated Vagrant file
Very little has changed other than some small syntax changes and the calls to the added Vagrant plugins.  I've taken the liberty of changing the box as well and pointing it to a box I made myself.  It's just [this box](https://github.com/jedi4ever/veewee/tree/master/templates/ubuntu-13.04-server-amd64) with the RubyGem version moved back to 1.8.25.

Why?  Because [reasons](https://github.com/rubygems/rubygems/issues/502).  I'd advise you make your own with [VeeWee](https://github.com/jedi4ever/veewee) to prevent those crazy Ruby guys from messing up your environment.  [Phil concurs](http://philsturgeon.co.uk/blog/2013/04/vagrant-and-chef-upgrade-party) and even has a pretty slick [tutorial](http://philsturgeon.co.uk/blog/2013/05/build-your-own-vagrant-boxes-with-veewee) on the subject.  


Vagrant / Chef Configurations
============

Everything that one needs to create development environments with Vagrant and Chef.

This repo is very much a work in progress. There are many things that can be improved. Annotations were made in comment form within the example Vagrantfile, regarding room for improvement. Issues and pull-requests are encouraged.

### Features Include

- LAMP (PHP 5.4)
- Git
- Composer
- PHPUnit
- automatic MySQL database creation
- automatic apache vhost creation
- other cookbooks and ready for love (ElasticSearch)
# Installation

These instructions are merely one way to use these tools. I'm presenting them because I feel that they're both reasonable and simple.

These are instructions for creating a development environment utilizing Vagrant for virtual-machine management and Chef for installing and configuring software.

You would install this individually into each site repo. When working on a site, you'd bring up the server. When you're done, you suspend it. I repeat. Each individual site repo has its own virtual-machine that is brought up when you're working on that specific site, and then suspended when you're done.

This should work on any Windows, OSX, or Linux box.

### First, install the applications.

**(DO NOT INSTALL FROM PACKAGE MANAGER)** If you do install virtualbox, vagrant or chef from package managers then expect pain.

- [Install VirtualBox](https://www.virtualbox.org/wiki/Downloads)

- [Install Vagrant(1.2.2)](https://www.virtualbox.org/wiki/Download_Old_Builds_4_2) Because [more reasons](http://serverfault.com/questions/518221/vagrant-up-fails-with-ns-error-call-failed-error)

- [Install Chef Client(11.4.4)](http://www.opscode.com/chef/install/)

### New Stuff

- [Install Librarian-Chef](https://github.com/applicationsonline/librarian-chef#how-to-use)

- [Install Vagrant-Omnibus](https://github.com/schisamo/vagrant-omnibus#installation)

- [Install Vagrant-Librarian-Chef](https://github.com/jimmycuadra/vagrant-librarian-chef#installation)
- **Make sure gem -v reads 1.x because 2.x breaks things due to Chef changes**



### Now, set up your project.

1. Add this repository to yours.

    $ cd mysite

    $ git submodule add git@github.com:WMeldon/vagrant-chef.git

2. ~~Update the submodules within the submodule. (inception)~~
 *This is no longer needed because of Librarian-Chef. Just vagrant up and you're good to go!*
    ~~$ git submodule update --init --recursive~~


3. Copy an example Vagrantfile to your project's root.

    $ cp vagrant-chef/vagrant/vagrantfiles/Vagrantfile .

4. Read the Vagrant file and modify where appropriate for your project.

5. Add this entry to hosts file

	10.10.10.10 app.local

# Workflow

### Start the Application

	$ vagrant up

Wait until Vagrant / Chef are done. Then, in your browser hit http://app.local.

### Stop the Application

You have the choice of either... suspending the application (takes a small bit more disk space). **Recommended**

	$ vagrant suspend

or.. you can halt the box. (saves like 200meg?) But, because next time you start it up again it'll take a long time.

	$ vagrant halt

### When making changes to the environment...

If configurations change you can simply run:

	$ vagrant reload

It will then apply your changes to the Vagrantfile or cookbooks.

### Easing the pain

Ok, it's not really that painful in exchange for what you get from it. I add these aliases to my shell script's initialization file for ease. You might like them, too.

    alias vl="VBoxManage list runningvms"
    alias vu="vagrant up"
    alias vd="vagrant suspend"
    alias vr="vagrant reload"
    alias vs="vagrant ssh"
    alias ga="git add ."
    alias gc="git commit -a"
    alias gp="git push"

### Troubleshooting

There is a known Mac issue with VirtualBox crashing your VMs. [Here are more details and the solution.](https://www.virtualbox.org/ticket/11649)

### How you can help!

I've made many annotations of issues in the Vagrantfile example, each could use a solution.

Trying to install this on your system and reporting back any issues that you've had with instructions listed would be a huge help.

Know a bit about Vagrant / Chef and want to complain that something could be done better? Please open an issue.

## Closing Thoughts

I decided to make this fork to get things up to date (a near impossible task in the DevOps world).  That said, I'm make no guarantees and am far from an expert.  But I thought this might be helpful to someone in a similar position.

**Thanks for the excellent work Shawn**
