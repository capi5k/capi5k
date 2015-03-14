Capi5k is a **proof of concept** for making the deployments on Grid'5000 easier, reusable and shareable.

It consists in a set of conventions over [Capistrano (v2)](https://github.com/capistrano),
and [bower](http://bower.io/). It also makes use of
[xp5k](https://github.com/pmorillo/xp5k) for job submissions and deployments
on [Grid'5000](https://grid5000.fr).

[Prerequisites >](https://github.com/capi5k/capi5k/wiki/Prerequisites)
[Wiki >](https://github.com/capi5k/capi5k/wiki)
[Project Home >](https://github.com/capi5k)

## Quick start (aka. quick setup)

### Initialization

* Initializa a new project :

```bash
$) xpm init hello
```

* This will download a skeleton project under the ```hello```directory.

Move to the project directory

```bash
$) cd hello
```
If you use [rvm](http://rvm.io) you'll move to a new gemset.

* Install runtime dependencies (capistrano, xp5k ...)

```bash
$) bundle install
```

### Validate the installation

* Run ``` cap -T ``` and you should see :

```
$)  cap -T
cap automatic # Automatic deployment
cap clean     # Remove all running jobs
cap deploy    # Deploy with Kadeploy
cap describe  # Describe the cluster
cap invoke    # Invoke a single command on the remote servers.
cap myproject # TODO : myproject task
cap shell     # Begin an interactive Capistrano session.
cap submit    # Submit jobs
```

* Then try to submit a job :

```
$) cap submit
  * 2014-05-11 11:42:37 executing `submit'
 ** Waiting for the job init #562301 to be running at nancy...
.. [OK]
```

See the file : ```config/deploy/xp5k.rb``` to see the description of the job.

If something went wrong here, check your *[restfully](http://github.com/crohr/restfully)* configuration.

* Then try to deploy your nodes :

```
$) cap deploy
  * 2014-05-11 11:44:38 executing `deploy'
 ** Waiting for all the deployments to be terminated...
............... [OK]
Summary of the deployment
------------------------------------------------------------
                Name            Deployed          Undeployed
------------------------------------------------------------
         capi5k-init                   2                   0
```

### First command

A capistrano role is defined in ```roles.rb```.

* Test the connection to your deployed nodes :

```
$) cap invoke COMMAND="date" ROLES="myrole" USER="root"
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

* You can define the command (and many others) inside the Capfile.

```ruby
namespace :myproject do

  desc 'TODO : myproject task'
  task :default do
    date
    # other tasks can be added here
  end

  desc 'run date command'
  task :date, :roles => [:myrole] do
    set :user,"root"
    run "date"
  end
end
```

You can launch the ```date``` task by invoking :

  * ```cap myproject```, this will call all the tasks written in the ```default``` block,
  * or simply ```cap myproject:date```


## Create a module  (aka. complete setup)

soon
