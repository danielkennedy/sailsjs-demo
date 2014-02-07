sailsjs-demo
============

This simple, default sails app demonstrates how to get sails implemented on 
Cloud Foundry PaaS.

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
normally use `sails lift`. On Cloud Foundry, you can't take for granted that
`sails` will be available in the runtime environment. We'll compensate for 
that here, by installing sails as a direct dependency for the application.

    $ npm install sails --save

Further, we need to prepare the application to listen on the port that is
assigned by Cloud Foundry. To do that, edit `config/local.js` as follows:

    port: process.env.VCAP_APP_PORT || 1337

Deployment
----------
Now that the stage is set, all we need to do is deploy the application. For
this demo app, I'm using [AppFog](http://appfog.com) and their cool `af` 
command line tool. The only major variance from the default setup is some 
additional memory, as I noticed that 64M wasn't quite enough.

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
