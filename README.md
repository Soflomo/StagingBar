Soflomo StagingBar
===

The Soflomo StagingBar is a very small Zend Framework 2 module rendering a notification bar at the top of every page. The bar is meant for staging environment to inform visitors the website is only meant for testing purposes and should not be confused with the production version.

![Preview of staging bar](https://raw.github.com/Soflomo/StagingBar/master/Preview.png)

Installation
---
Add "soflomo/staging-bar" to your composer.json file and update your dependencies. Enable
Soflomo\StagingBar in your local `application.config.php`.

If you do not have a composer.json file in the root of your project, copy the
contents below and put that into a file called `composer.json` and save it in
the root of your project:

```
{
    "require": {
        "soflomo/staging-bar": ">=1.0.0,<2.0.0-dev"
    }
}
```

Then execute the following commands in a CLI:

```
curl -s http://getcomposer.org/installer | php
php composer.phar install
```

Now you should have a `vendor` directory, including a `soflomo/staging-bar`. In your
bootstrap code, make sure you include the `vendor/autoload.php` file to properly
load the Soflomo StagingBar module.

Local application configuration
---
This module is the easiest to load via a local application configuration file. This means that just like "local" files in the `config/autoload`, you can also enable application on a local level. To achieve this, replace your `application.config.php` with the following file:

```
use Zend\Stdlib\ArrayUtils;

$config = array(
    'modules' => array(
        // Here all your global modules
    ),
    'module_listener_options' => array(
        'config_glob_paths'    => array(
            'config/autoload/{,*.}{global,local}.php',
        ),
        'module_paths' => array(
            './module',
            './vendor',
        ),
    ),
);

$local = __DIR__ . '/application.config.local.php';
if (is_readable($local)) {
    $config = ArrayUtils::merge($config, require($local));
}

return $config;
```

Then create a local `dist` file which acts as a template for all local installations, called `application.config.local.php.dist`:

```
return array(
    'modules' => array(
        // Here all your local modules
    ),
);
```

Then copy this dist file to remove the `.dist` extension. Commit the new application config and dist file into your versioning system and make sure to ignore the local application configuration.

Now you can easily enable `Soflomo\StagingBar` in your local application configuration without the struggle to disable it on your development or production versions.

Custom staging bar
---
You can override the template used to render this bar. Just create a view script that resolves to `soflomo-staging-bar/partial/staging-bar.phtml` and that file will be rendered. Note the bar contains some inline styles to render the bar correctly. If you override the template, it is suggested to copy these styles as well.

If you do not want to render a script with this long template name, you can use the `template` key in the configuration to modify the location of the template:

```
'soflomo_staging_bar' => array(
    'template' => 'here/your/template'
),
```

Custom variables
---
In your own template, you can inject variables to ease configuration of the bar. The `variables` key in the configuration is mapped to variables in the view model:


```
'soflomo_staging_bar' => array(
    'variables' => array('foo' => 'bar'),
),
```

If you call `$foo` in your template, it will echo `bar`.
