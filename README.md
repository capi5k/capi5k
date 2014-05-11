Capi5k is a **proof of concept** for making the deployments on Grid'5000
(and beyond) easier, reusable and shareable.
It is based on the use of modules (like nodejs modules, gems ...),
each module will install and configure something on nodes.
You can write your own modules and publish them so that everyone can reuse them.

It consists in a set of convention over [Capistrano (v2)](https://github.com/capistrano),
and [npm](https://www.npmjs.org/). It also makes use of
[xp5k](https://github.com/pmorillo/xp5k) for job submssion and deployment
on [Grid'5000](https://grid5000.fr).


# Getting started

## Prerequisites


* ruby environment. Use rvm or rbenv and bundler.
* npm : nodejs package manager is used to manage modules dependencies
* restfully configured :

``` yaml
$) cat ~/.restfully/api.grid5000.fr.yml
base_uri: https://api.grid5000.fr/3.0
username: "###"
password: "###"
```


## Your first project


Download the skeleton of a project.
It can be achieve by downloading the tarball / zip or cloning the repository
```capi5k-init```. See the links in the header.

Inside the repository, install the gem dependencies (Capistrano, Xp5k ...)

```
capi5k-init ➤ capi5k-init
```


We assume that bundle is installed.
We encourage to use rvm or rbenv to manage your ruby environment.
```
capi5k-init ➤ bundle install
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
capi5k-init ➤ tree -L 2                                                                                                                                                                          git:master
.
├── Capfile         # entry point of the project
├── Gemfile         # manage ruby dependencies
├── README.md       # readme
├── config
│   ├── deploy      # contains xp5k deployment files
│   ├── deploy.rb   # load capi5k dependency (do not edit it)
│   └── lib         # ruby lib
├── config.rb       # specific parameters (ssh keys, walltime, site)
└── package.json    # description of the capi5k project + dependencies declaration
```

### SSH keys

Change the ```config.rb``` file accordingly to your parameters.

Note that with the gateway set, you will run the scripts directly from your local computer.

### Test your installation

* Run ``` cap -T ``` and you should see :

```
capi5k-init ➤ cap -T                                                                                                                                                                            git:master*
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
capi5k-init ➤ cap submit                                                                                                                                                                        git:master*
  * 2014-05-11 11:42:37 executing `submit'
 ** Waiting for the job init #562301 to be running at nancy...
.. [OK]
```

See the file : ```config/deploy/xp5k.rb``` to see the description of the job.

If something wrong here, check your *restfully* configuration.

* Then try to deploy your nodes :

```
capi5k-init ➤ cap deploy                                                                                                                                                                        git:master*
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

* Open the ```Capfile```and aadd the following :

```ruby
role :test do
  $myxp.get_deployed_nodes('capi5k-init')
end
```

* Test the connection to your deployed nodes :

````
capi5k-init ➤ cap invoke COMMAND="date" ROLES="test" USER="root"                                                                                                                                git:master*
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
