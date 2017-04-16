FooI18n
=======

Library to make using translations in Opulence easy.


Setup
-----

Install the library via composer:

```
composer install peteraba/foo-i18n
```

Add the bootstrapper to you application:
```php
# config/bootstrappers.php

return [
    RequestBootstrapper::class,
    // ...
    Foo\I18n\Bootstrapper\I18nBootstrapper::class,
];
```

Add your translations in `resources/lang` as it already exist for validation.

Add your default language to your `.env.app.php`:
```
Environment::setVar('DEAFULT_LANGUAGE', "hu");
```


Usage
-----

Files under `resources/lang/${DEFAULT_LANG}` will be loaded automatically. Values defined in the language files are
_namespaced_ by a `:` character, so the value *mainPageTitle* defined in `application.php` can be referenced as *application:mainPageTitle*

User classes can access the translator via loading from the [IOC Container](https://www.opulencephp.com/docs/1.0/ioc-container) as *ITranslator*.

In [Fortune](https://www.opulencephp.com/docs/1.0/view-fortune) you can call the helper *tr* to retrieve your translations.


Example
-------

**resources/lang/en/form.php**
```php
<?php

return [
    'createNewLabel' => 'Create new %s',
];
```

**src/Project/Form/Login**
```php
class Login {
    public function __construct(ITranslator $translator, string $entityName)
    {
        $this->translator = $translator;
        $this->entityName = $entityName;
    }
    public function createSaveButton(): Button
    {
        return new Button($this->translator('form:createNewLabel'. $this->entityName);
    }
}
```

**resources/views/forms/login.fortune.php**
```php
<button>{{ tr("form::createNewLabel", $entityName) }}</button>
```


Notes
-----

1. The library will default to use *en* as default language if one is not provided.
2. The bootrapper is not Lazy loaded, because international application usually need translations throughout the application.
3. At the moment translations are not cached, but that's a planned feature.

