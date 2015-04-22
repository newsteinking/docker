.. _`LinuxCMD`:

chapter 4 :AngularJS
============================


4.1 Basic
------------------------



4.1.1 mastering angularjs web application
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

01 - hello world

cd 01\ -\ hello\ world/







4.2 Extension
------------------------

npm install
npm install express






4.2.1 AngularJS +Express+NodeJS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

ref : http://briantford.com/blog/angular-express

https://github.com/btford/angular-express-seed

https://github.com/angular/angular-seed


body-parser warning

::

    //app.use(bodyParser());
    //app.use(bodyParser.urlencoded());
    app.use(bodyParser.urlencoded({ extended: true }));
    app.use(bodyParser.json());

.

run: npm install express-error-handler
change line 9 to: errorHandler = require('express-error-handler'),
change line 36 to: app.use(errorHandler());

::

    npm install express-error-handler

app.js
::

    //  errorHandler = require('error-handler'),
    errorHandler = require('express-error-handler'),

    //app.use(bodyParser());
    //app.use(bodyParser.urlencoded());
    app.use(bodyParser.urlencoded({ extended: true }));
    app.use(bodyParser.json());


    //app.use(methodOverride());
    app.use(methodOverride());

    //  app.use(express.errorHandler());
    app.use(errorHandler());

.

4.2.2 generator-angular-fullstack
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


https://github.com/DaftMonk/generator-angular-fullstack

*cache clean

npm cache clean
bower cache clean



root:

::

    npm install -g generator-angular-fullstack



sean:
::

    mkdir my-new-project && cd $_
    yo angular-fullstack [app-name]

.
Run grunt for building, grunt serve for preview, and grunt serve:dist for a preview of the built app.








4.2.3 npm proxy setting
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

npm proxy setting
::

    npm config set proxy http://xx.xx.xx.xx:8080
    npm config set https-proxy http://xx.xx.xx.xx:8080
    npm config set strict-ssl false

.

4.2.4 yoeman
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

https://github.com/yeoman/generator-angular


in root

npm install -g grunt-cli bower yo generator-karma generator-angular

in sean

mkdir my-new-project && cd $_

yo angular [app-name]

4.2.5 malhar-dashboard-webapp
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
https://github.com/DataTorrent/malhar-dashboard-webapp



https://github.com/the-lay/zabbix-angularjs

sudo npm install  -g grunt-cli

npm install

grunt

grunt serve