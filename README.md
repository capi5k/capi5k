Capi5k is a **proof of concept** for making the deployments on Grid'5000
(and beyond) easier, reusable and shareable.

It consists in a set of conventions over [Capistrano (v2)](https://github.com/capistrano),
and [bower](http://bower.io/). It also makes use of
[xp5k](https://github.com/pmorillo/xp5k) for job submissions and deployments
on [Grid'5000](https://grid5000.fr).

[Prerequisites >](https://github.com/capi5k/capi5k/wiki/Prerequisites)
[Wiki >](https://github.com/capi5k/capi5k/wiki)
[Project Home >](https://github.com/capi5k)

## Getting started


Initialize a new project : 

```
xpm new myproject
```

We assume that bundle is installed.
We encourage to use rvm or rbenv to manage your ruby environment.
```
myproject ➤ bundle install
[...]
Using term-ansicolor (1.3.0)
Using xp5k (0.0.5) from https://github.com/msimonin/xp5k.git (at redeploy)
Using bundler (1.5.3)
Your bundle is complete!
Use `bundle show [gemname]` to see where a bundled gem is installed.

```

You're done with the installation.

## Setup


```
myproject ➤ tree -L 2                                                                                                                                                                                      
.
├── Capfile
├── Gemfile
├── README.md
├── bower.json
├── config
│   ├── deploy
│   ├── deploy.rb
│   └── lib
├── config.rb
├── output.rb
├── recipe.rb
├── roles.rb
└── roles_definition.rb
```

### SSH keys

Change the ```config.rb``` file accordingly to your parameters.

Note that with the gateway set, you will run the scripts directly from your local computer.

### Test your installation

* Run ``` cap -T ``` and you should see :

```
myproject ➤ cap -T                                                                                                                                                                            
cap automatic # Automatic deployment
cap clean     # Remove all running jobs
cap deploy    # Deploy with Kadeploy
cap describe  # Describe the cluster
cap invoke    # Invoke a single command on the remote servers.
cap shell     # Begin an interactive Capistrano session.
cap submit    # Submit jobs

Some tasks were not listed, either because they have no description,
or because they are only used internally by other tasks. To see all
tasks, type `cap -vT'.

Extended help may be available for these tasks.
Type `cap -e taskname' to view it.
```

* Then try to submit a job :

```
myproject ➤ cap submit                                                                                                                                                                       
  * 2014-05-11 11:42:37 executing `submit'
 ** Waiting for the job init #562301 to be running at nancy...
.. [OK]
```

See the file : ```config/deploy/xp5k.rb``` to see the description of the job.

If something wrong here, check your *restfully* configuration.

* Then try to deploy your nodes :

```
myproject➤ cap deploy                                                                                                                                                                        
  * 2014-05-11 11:44:38 executing `deploy'
 ** Waiting for all the deployments to be terminated...
............... [OK]
Summary of the deployment
------------------------------------------------------------
                Name            Deployed          Undeployed
------------------------------------------------------------
         capi5k-init                   2                   0
```

### Add a role

* Open the ```Capfile```and add the following :

```ruby
role :test do
  $myxp.get_deployed_nodes('capi5k-init')
end
```

* Test the connection to your deployed nodes :

```
myproject ➤ cap invoke COMMAND="date" ROLES="test" USER="root"                                                                                                                                
  * 2014-05-11 11:54:41 executing `invoke'
  * executing multiple commands in parallel
    -> "else" :: "date"
    -> "else" :: "date"
    servers: ["graphite-3.nancy.grid5000.fr", "graphite-4.nancy.grid5000.fr"]
  * establishing connection to gateway `"msimonin@access.grid5000.fr"'
  * Creating gateway using msimonin@access.grid5000.fr
Enter passphrase for /Users/msimonin/.ssh/id_rsa:
  * establishing connection to `graphite-3.nancy.grid5000.fr' via gateway
  * establishing connection to `graphite-4.nancy.grid5000.fr' via gateway
    [graphite-3.nancy.grid5000.fr] executing command
    [graphite-4.nancy.grid5000.fr] executing command
 ** [out :: graphite-3.nancy.grid5000.fr] Sun May 11 11:54:38 CEST 2014
 ** [out :: graphite-4.nancy.grid5000.fr] Sun May 11 11:54:32 CEST 2014
    command finished in 270ms
```

# Add dependencies to your project

The dependencies are for the moment available as git repo. 
Avalaible modules : 

* hadoop
* serf
* nfs
* cassandra
* puppet
* ?

You can check : https://github.com/capi5k

# Future


There is so much to do ! but 

* to have a decent documentation
* ?

can participate to improve the idea of capi5k.


