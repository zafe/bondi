# CodeIgniter on OpenShift #
This QuickStart was created to make it easy to get started with CodeIgniter 2 
on OpenShift.

[CodeIgniter](http://www.codeigniter.com/) is a powerful PHP 
framework with a very small footprint, built for developers who need a simple 
and elegant toolkit to create full-featured web applications. CodeIgniter 2.x
is the legacy version of the framework. The current version (2.2.1) came 
out in January, 2015.

The simplest way to install this application is to use the [OpenShift
QuickStart](https://hub.openshift.com/quickstarts/16-codeigniter). If 
you'd like to install it manually, follow [these directions](#manual-installation).

## OpenShift Considerations ##
These are some special considerations you may need to keep in mind when running 
your application on OpenShift using this QuickStart.

### .htaccess Configuration ###
The `.htaccess` file has been pre-configured to remove trailing slashes and 
index.php from URLs (based on typical CodeIgniter best-practices).

### Handling Assets (CSS, fonts, images, and JavaScript) ###
Deciding where to store your asset files isn't clear with CodeIgniter.
Accordingly, this QuickStart is pre-configured with an `assets` directory for
you to store your custom fonts, images, javascripts, and stylesheets.

We've also extended the base HTML and URL helper files to make using assets stored 
in the assets directories easy.

#### URL Helper ####
The URL Helper file contains functions that assist in working with URLs.

_ADDED_ **Asset URL** - returns the URL of your assets directory. Example:

    echo asset_url();
    // http://example.com/assets/

Or, with a URI:

    echo asset_url('images/logo.png'); 
    // http://example.com/assets/images/logo.png

#### HTML Helper ####
The HTML Helper file contains functions that assist in working with HTML.

_UPDATED_ **Img** - Lets you create HTML <img /> tags. Updated to automatically 
prepend 'assets/images/' to path when necessary. Example:

    echo img('picture.jpg');
    // <img src="http://example.com/assets/images/picture.jpg" />

_UPDATED_ **Link Tag** - Lets you create HTML <link /> tags (mostly for stylesheets). Updated 
to automatically prepend 'assets/stylesheets/' to path when necessary and 
rel="stylesheet". 
Example:

    echo link_tag('styles.css');
    // <link src="http://example.com/assets/stylesheets/styles.css" rel="stylesheet" type="text/css" />

_ADDED_ **Script Tag** - Lets you create HTML <script /> tags. Automatically
prepends 'assets/javascripts/' to path when necessary. Example:

    echo script_tag('jquery.js');
    // <script type="text/javascript" src="http://example.com/assets/javascripts/jquery.js"></script>

### Local vs. Remote Development ###
This CodeIgniter QuickStart provides separate configuration files for both local 
and remote development, found in `application/config/local` and 
`application/config` respectively.

#### Local Development ####
When developing locally, this QuickStart will set the CodeIngiter `ENVIRONMENT` 
to 'local' development mode.

Configuration files for working locally can be found at 
`application/config/local`. See the CodeIngiter user guide for more information 
on how CodeIgniter handles 
[Multiple Environments](http://www.codeigniter.com/user_guide/general/environments.html).

#### Remote Development ####
Your application is configured to automatically use your OpenShift MySQL or 
PostgreSQL database in when deployed on OpenShift by making use of
[OpenShift Environment Variables](https://developers.openshift.com/en/managing-environment-variables.html).

The CodeIgniter `base_url` and `encryption_key` have also been configured to 
automatically draw from OpenShift Environment variables.

OpenShift provides a directory for data that requires persistent storage.
Accordingly, the CodeIgniter `cache_path`, database `cachedir`,  and 
`session_save_path` have been set to use this directory for storage.

Finally, the CodeIgniter `ENVIRONMENT` will be set automatically in 
production on OpenShift to match the value of the `APPLICATION_ENV` OpenShift 
Environment variable, set to 'production' by default.

##### Development Mode #####
When you develop your CodeIgniter application on OpenShift, you can also enable 
the 'development' environment by setting the `APPLICATION_ENV` environment 
variable using the `rhc` client, like:

```
$ rhc env set APPLICATION_ENV=development -a <app-name>
```

Then, restart your application:

```
$ rhc app restart -a <app-name>
```

If you do so, OpenShift will run your application under 'development' mode.
In development mode, your application will:

* Set CodeIgniter's `ENVIRONMENT` to 'development'
* Show more detailed errors in browser
* Display startup errors
* Enable the [Xdebug PECL extension](http://xdebug.org/)
* Enable [APC stat check](http://php.net/manual/en/apc.configuration.php#ini.apc.stat)
* Ignore your composer.lock file

Set the variable to 'production' and restart your app to deactivate error reporting 
and resume production PHP settings.

Using the development environment can help you debug problems in your application
in the same way as you do when developing on your local machine. However, we 
strongly advise you not to run your application in this mode in production.

### Log Files ###
Your application is configured to use the OpenShift log directory. You can use
the `rhc tail` command to stream the latest log file entries:

```
rhc tail -a <APP_NAME>
```

To stop tailing the logs, press *Ctrl + c*.

## Manual Installation ##

1. Create an account at https://www.openshift.com/

1. Create a CodeIgniter application:

    ```
    rhc app create codeigniterapp php-5.4 mysql-5.5 --from-code=https://github.com/openshift/CodeIgniterQuickStart
    ```
    or

    ```
    rhc app create codeigniterapp php-5.4 postgresql-9.2 --from-code=https://github.com/openshift/CodeIgniterQuickStart
    ```

## Additional Resources ##
Documentation for the CodeIgniter framework can be found on the 
[CodeIgniter website](http://www.codeigniter.com/user_guide/). Check out 
OpenShift's [Developer Portal](https://developers.openshift.com/en/php-overview.html) 
for help running PHP on OpenShift.