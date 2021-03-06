Environment Setup
=================

1. Introduction
2. Register your environment
3. Shared environment configuration
4. Install WordPress

1.Introduction
--------------

The Themosis framework has its own way for defining WordPress configurations. It's done on purpose to facilitate collaboration. By default, the Themosis framework comes with a `local` and a `production` environments pre-configured.

You'll start by registering your database credentials and application URLs into a `.env.{environment}.php` file located in the root directory of your project. Then you'll be able to define your environment configurations by modifying files located in the `config` directory.

> Opening your project in a text editor or IDE should show you two default `.env` files: `.env.local.php`, `.env.production.php`.

2.Register your environment
--------------------------

Let's start by installing your WordPress application on a local environment.

>Follow the same steps for a remote/production environment or any custom ones.

#### 1 - Set your credentials and URLs

Open the default `.env.local.php` file located in the root of your project. Fill in the values with your local database credentials and specify your local virtual host URLs.

```php
<?php

/*----------------------------------------------------*/
// Local environment vars
/*----------------------------------------------------*/
return array(

    'DB_NAME'		=> 'your_database_name',
    'DB_USER'		=> 'database_username',
    'DB_PASSWORD'	=> 'database_password',
    'DB_HOST'		=> 'localhost',
    'WP_HOME'		=> 'http://my-website.dev',
    'WP_SITEURL'	=> 'http://my-website.dev/cms'

);
```

> WordPress is defined as a dependency and is loaded by the framework inside the `cms` directory located in the web root folder `htdocs`. Make sure to always define the `WP_SITEURL` value with `/cms` appended at the end.

> The `DB_NAME`, `DB_USER`, `DB_PASSWORD`, `DB_HOST`, `WP_HOME`, `WP_SITEURL` variables are all required. You can retrieve those values any time by using the PHP function `getenv('DB_NAME');`.

Once your credentials are registered, we need to identify the local environment.

#### 2 - Identify your local environment

In order for the framework to identify your `local` environment, you have to register your machine/computer `hostname` in the `environment.php` file located inside the `config` directory.

##### Find hostname on *NIX

Open your Terminal and simply run the following command:
```bash
hostname
```

##### Find hostname on Windows

Open your Console and run the following command:
```bash
ipconfig/all
```

Look at the first displayed line `Host Name`.

Once you get your hostname, open the `environment.php` file and replace the value of the `local` key:

```php
<?php

/*----------------------------------------------------*/
// Define your environments
/*----------------------------------------------------*/
return array(

    'local'             => 'WRITE YOUR HOSTNAME HERE',
    'production'        => 'remote machine hostname'

);
```

> The key defined in the `environment.php` file is used to load the `.env.{$environment}.php` file and also the `{$environment}.php` file located in the `config/environments` directory.

##### Add configurations

By default, the Themosis framework defines a `local` environment and loads the default `local.php` file stored in the `config/environments` folder:

```php
<?php

/*----------------------------------------------------*/
// Local config
/*----------------------------------------------------*/
// Database
define('DB_NAME', getenv('DB_NAME'));
define('DB_USER', getenv('DB_USER'));
define('DB_PASSWORD', getenv('DB_PASSWORD'));
define('DB_HOST', getenv('DB_HOST') ? getenv('DB_HOST') : 'localhost');

// WordPress URLs
define('WP_HOME', getenv('WP_HOME'));
define('WP_SITEURL', getenv('WP_SITEURL'));

// Development
define('SAVEQUERIES', true);
define('WP_DEBUG', true);
define('SCRIPT_DEBUG', true);

// Themosis framework
define('THEMOSIS_ERROR_DISPLAY', true);
define('THEMOSIS_ERROR_SHUTDOWN', true);
define('THEMOSIS_ERROR_REPORT', -1);
```
This is the pre-configured local configuration file. You can add as many as you want configurations to this file. These are only available for your `local` environment.

3.Shared environment configuration
----------------------------------

Some WordPress configurations are the same whatever environment you're using: WP_LANG, Authentication salts,...

Common configuration can be found into the `shared.php` file stored in the `config` directory.

In order to define shared configuration, you can edit this `shared.php` file. Fill in default configuration or add custom ones.

In order to finish our local configuration, open the `shared.php` file and fill in the authentication salts:

```php
<?php

/*----------------------------------------------------*/
// Authentication unique keys and salts
/*----------------------------------------------------*/
/**
 * @link https://api.wordpress.org/secret-key/1.1/salt/ WordPress.org secret-key service 
 */
define('AUTH_KEY',         'put your unique phrase here');
define('SECURE_AUTH_KEY',  'put your unique phrase here');
define('LOGGED_IN_KEY',    'put your unique phrase here');
define('NONCE_KEY',        'put your unique phrase here');
define('AUTH_SALT',        'put your unique phrase here');
define('SECURE_AUTH_SALT', 'put your unique phrase here');
define('LOGGED_IN_SALT',   'put your unique phrase here');
define('NONCE_SALT',       'put your unique phrase here');

```

4.Install WordPress
-------------------

Once your environment is setup, open your browser and start the default WordPress installation process. Log in your WordPress administration, activate the Themosis framework plugin and the Themosis framework theme.

Visit your project home page and you should be granted with a welcome message. Congratulations! You have installed WordPress and the Themosis framework.

> Make sure to set write permissions on the `storage` folder located inside the `app` directory of the Themosis framework theme.

Next
----
Read the [framework guide](http://framework.themosis.com/docs/framework/)