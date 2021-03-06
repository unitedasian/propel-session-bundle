UAMPropelSessionBundle
======================

UAMPropelSessionBundle is an opinitionated symfony bundle that provides a convenient implementation of sessions via Propel ORM.

The motivation for this bundle was an issue of inconvenience:

* Configuring sessions in multiple symfony apps is repetitive, yet so standardized as not worth the effort each time.
* The session table does not really need a schema nor Propel OM classes in the app; yet if not present, the `propel:migration:generate-diff` will attempt to remove it each time it is run.

To resolve this, this bundle provides a ready-to-use configuration for the app's sessions-related settings, as well as a Propel schema for the sessions table.

This bundle is highly opinionated: 

It provides no latitude for customization; if the settings it provides are unsuitable for your app, then you should customize the session settings the usual way in your app's configuration. 

This bundle also deliberately ignores basic principles of software design, and assumes the existence of certain named container parameters; if these parameters are absent, the app may fail cryptically. On the other hand, these parameters are usually present in any symfony apps; changing their names in the app config to match the names exeepcted by the bundle presents little difficulty.

This bundle uses the default table name for sessions (`sessions`) and default column names (`sess_id`, `sess_data`, `sess_time`, `sess_lifetime`). 

The following parameters should be defined somewhere in the app:

* `database_host`
* `database_port`
* `database_name`
* `database_user`
* `database_password`

Usage 
-----

Add the bundle to your app's `composer.json` file requirements:

```json
    require: {
    	"unitedasian/propel-session-bundle": "~1.0",
    	...
	}
```

Enable the bundle in your app's kernel:

```php
# app/AppKernel.php

    public function registerBundles()
    {
		$bundles = array(
            new UAM\Bundle\PropelSessionBundle\UAMPropelSessionBundle(),
            ...
        );

        ... 

        return $bundles;
    }
```

Create the `sessions` table: There are plenty of ways to do so via a propel command, such as `propel:sql:build`; the easiest method is probably to run `propel:migration:generate-diff` and then `propel:migration:migrate`.

Update your app's configuration to use the session handler service provided by the bundle:

```yaml
# app/config/config.yml

framework:
    session:
        handler_id: unitedasian.session.handler.pdo
```
