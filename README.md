sailsjs-demo
============

This simple, default [Sails](http://sailsjs.org/) app demonstrates how to get Sails implemented a on 
Cloud Foundry-based PaaS.

Setup
-----
If you don't already have `sails` installed, you'll need to get it from npm (and
install it as a globally available command). Once installed, you can use `sails
new <appname>` to create a new scaffolded application.

    $ sudo npm install sails -g
    $ sails new sailsjs-demo
    $ cd sailsjs-demo

Secret Sauce
------------
In order to get your new application to operate on your local system, you'd 
normally use `sails lift`. On Cloud Foundry, npm should install `sails` during deployment, but you will still want to specify a start command for your application.

Further, we need to prepare the application to listen on the port that is
assigned by Cloud Foundry. To do that, create and edit `config/local.js` as follows:

    port: process.env.VCAP_APP_PORT || 1337

Deployment
----------
Now that the stage is set, all we need to do is deploy the application. The only major variance from the default setup is some additional memory, as the default 64M isn't quite enough.

Using [Pivotal Web Services](http://run.pivotal.io) and the latest `cf` v6 [CLI tool](https://console.run.pivotal.io/download_cli) - setting a start command, and allocating plenty of memory to avoid an out of memory error on the server side.

````
$ cf push -c "sails lift" -m 150M sailsdemo
Creating app sailsdemo in org apiper-org / space development as apiper@gopivotal.com...
OK

Using route sailsdemo.cfapps.io
Binding sailsdemo.cfapps.io to sailsdemo...
OK

Uploading sailsdemo...
Uploading from: /Users/andyp/Development/github/sailsjs-demo
348.6K, 105 files
OK

Starting app sailsdemo in org apiper-org / space development as apiper@gopivotal.com...
OK
-----> Downloaded app package (240K)


       PRO TIP: Specify a node version in package.json
       See https://devcenter.heroku.com/articles/nodejs-support
-----> Defaulting to latest stable node: 0.10.25
-----> Downloading and installing node
-----> Installing dependencies
       npm http GET https://registry.npmjs.org/optimist/0.3.4
[snip npm installation output]
-----> Caching node_modules directory for future builds
-----> Cleaning up node-gyp and npm artifacts
-----> No Procfile found; Adding npm start to new Procfile
-----> Building runtime environment
-----> Uploading droplet (18M)

1 of 1 instances running

App started

Showing health and status for app sailsdemo in org apiper-org / space development as apiper@gopivotal.com...
OK

requested state: started
instances: 1/1
usage: 150M x 1 instances
urls: sailsdemo.cfapps.io

     state     since                    cpu    memory           disk
#0   running   2014-02-07 11:36:49 AM   0.0%   125.7M of 150M   68M of 1G
````

Using [AppFog](http://appfog.com) and their `af` 
command line tool. 

````
$ af push sailsjs-demo
Would you like to deploy from the current directory? [Yn]: 
Detected a Node.js Application, is this correct? [Yn]: 
1: AWS US East - Virginia
2: AWS EU West - Ireland
3: AWS Asia SE - Singapore
4: HP AZ 2 - Las Vegas
5: Savvis US East
6: Savvis UK Central
7: Savvis Germany Central
Select Infrastructure: 1
Application Deployed URL [sailsjs-demo.aws.af.cm]: 
Memory reservation (128M, 256M, 512M, 1G, 2G) [64M]: 128M
How many instances? [1]: 
Bind existing services to 'sailsjs-demo'? [yN]: 
Create services to bind to 'sailsjs-demo'? [yN]: 
Would you like to save this configuration? [yN]: 
Creating Application: OK
Uploading Application:
  Checking for available resources: OK
  Processing resources: OK
  Packing application: OK
  Uploading (275K): OK   
Push Status: OK
Staging Application 'sailsjs-demo': OK                                          
Starting Application 'sailsjs-demo': OK
````
