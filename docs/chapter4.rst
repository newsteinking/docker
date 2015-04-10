.. _`LinuxCMD`:

chapter 4 :AngularJS
============================

4.1 Basic
------------------------

npm install
npm install express






4.1.1 Directory Size
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
https://github.com/btford/angular-express-seed


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

4.1.2 manual core dump
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

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






