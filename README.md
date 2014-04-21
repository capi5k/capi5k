Capi5k
=======

This is a sample of what capi5k can do.

Installation
============

Clone the repository, it can be used as a skeleton for your future project.

```
$) git clone https://github.com/capi5k/capi5k.git 
```

Inside the repository, install the dependencies.

First the needed gems, we assume that bundle is installed. We encourage to use rvm or rbenv to manage your ruby environment.

``` 
$)cd capi5k 
```

```
$)bundle install
[...]
Using term-ansicolor (1.3.0)
Using xp5k (0.0.5) from https://github.com/msimonin/xp5k.git (at redeploy)
Using bundler (1.5.3)
Your bundle is complete!
Use `bundle show [gemname]` to see where a bundled gem is installed.

```

Then install the needed modules for the project, we assume that ``` npm ``` (node package manager) is installed. 
The following will install all the dependencies specified in the file ```package.json```.

```
$) npm install
rabbitmq@0.0.1 node_modules/rabbitmq
```

You're done with the installation.

Usage
=====

Check the available tasks : 
```
$)cap -T

cap automatic # Automatic deployment
cap clean     # Remove all running jobs
cap deploy    # Deploy with Kadeploy
cap describe  # Describe the cluster
cap invoke    # Invoke a single command on the remote servers.
cap rabbitmq  # Deploy RabbitMQ on nodes
cap shell     # Begin an interactive Capistrano session.
cap submit    # Submit jobs
```
* automatic is defined in Capfile
* clean is an xp5k common task, see ```config/deploy/xp5k_common_tasks.rb``` loaded from ```config/deploy/xp5k.rb``` 
* deploy is an xp5k common task  
* describe is an xp5k common task
* invoke is capistrano built-in task
* rabbitmq is an imported task (more precisely namespace), loaded from ```config.deploy.rb```
* invoke is capistrano built-in task
* deploy is an xp5k common task

According to the Capfile launching ```cap automatic``` will install rabbitmq on the deployed nodes.

Roles
=====

Each module is shipped with predefined roles. For instance the rabbitmq module needs only one role wich define what nodes should have rabbitmq installed.
It can be found in the ```roles.rb``` of the module (in node_modules/rabbitmq/roles.rb). By default rabbitmq will be installed on nodes belonging to an xp5k roles or job with name ```rabbitmq```.

Since we deploy using ```config/deploy/xp5k``` wich defined a job with name ```sample```, we need to override the role ```rabbitmq``` by creating a ```roles.rb``` file in the root directory of the projectory.

One situation where it is worth to override the default role is when working with static hostnames (for example outside grid'5000), you could have something like :
$) cat roles.rb
role :rabbitmq do
  %w(hostname1, hostname2)
end

