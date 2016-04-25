Description
===========

Salt Sandbox is a multi-VM [Vagrant](http://vagrantup.com/)-based
[Salt](http://saltstack.org/) development environment used for creating
and testing new Salt state modules outside of your production environment.
It's also a great way to learn firsthand about Salt and its remote
execution capabilities.

Salt Sandbox will set up three separate virtual machines:

* _salt.example.com_ - the Salt master server
* _minion1.example.com_ - the first Salt minion machine
* _minion2.example.com_ - the second Salt minion machine

These VMs can be used in conjunction to segregate and test your modules
based on node groups, top file environments, grain values, etc. You can
even test modules on different Linux distributions or release versions to
better match your production infrastructure.

Requirements
============

To use Salt Sandbox, you must have the following items installed and
working:

* [VirtualBox](https://www.virtualbox.org/)
* [Vagrant](http://vagrantup.com/)

Salt Sandbox has been designed for and tested with Vagrant base boxes
running:

* CentOS 6.7
* Ubuntu 14.04.3 LTS

...although it may work just fine with other distributions/versions.

Usage
=====

    $ git clone git://github.com/cockcrow/salt-sandbox.git
    $ cd salt-sandbox/

Initial Startup
---------------

To bring up the Salt Sandbox environment, issue the following command:

    $ vagrant up

The following tasks will be handled automatically:

1. The Salt master daemon will be installed and enabled on the master machine.
2. The Salt minion daemon will be installed and enabled on all three machines.
3. A host-only network will be set up with all machines knowing how to
   communicate with each other.
4. The master server will utilize the `top.sls` file and `salt_roots/` 
   directory that exist **outside of the VMs** (in your salt-sandbox Git 
   working directory) by utilizing VirtualBox's shared folder feature.

If you wish to change the domain name of the VMs (it defaults to
_example.com_), edit the "domain" variable at the top of `Vagrantfile` and
reload the machines:

    $ vim Vagrantfile
    $ vagrant reload

Developing New Modules
----------------------

To start developing a new SLS module, just create the standard module structure
under `salt_roots/salt/` in your salt-sandbox Git working directory. This 
directory is automatically in the Salt master server's _file\_roots_ path, and 
any changes will be picked up immediately.

    $ mkdir -p salt_roots/salt/mymodule
    $ vim salt_roots/salt/mymodule/init.sls

To have your module actually applied to one or more of the minions, edit
the `top.sls` file and specify how it should be used during state
execution...that's it!

Check Your Handiwork
--------------------

To log on to the virtual machines and see the result of your Salt modules, just
use standard [Vagrant Multi-Machine Environment](http://docs.vagrantup.com/v2/multi-machine/index.html)
commands, and provide the proper VM name (`master`, `minion1`, or `minion2`):

    $ vagrant ssh master

Then instruct all minions to execute a highstate call and apply any applicable
modules:

    [vagrant@master ~]$ sudo salt '*' state.highstate

License
=======

Salt Sandbox is provided under the terms of [The MIT
License](http://www.opensource.org/licenses/MIT).

Copyright &copy; 2012, [Aaron Bull Schaefer](mailto:aaron@elasticdog.com).
