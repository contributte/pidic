![](https://heatbadger.vercel.app/github/readme/contributte/pidic/?deprecated=1)

<p align=center>
    <a href="https://bit.ly/ctteg"><img src="https://badgen.net/badge/support/gitter/cyan"></a>
    <a href="https://bit.ly/cttfo"><img src="https://badgen.net/badge/support/forum/yellow"></a>
    <a href="https://contributte.org/partners.html"><img src="https://badgen.net/badge/sponsor/donations/F96854"></a>
</p>

<p align=center>
    Website 🚀 <a href="https://contributte.org">contributte.org</a> | Contact 👨🏻‍💻 <a href="https://f3l1x.io">f3l1x.io</a> | Twitter 🐦 <a href="https://twitter.com/contributte">@contributte</a>
</p>

## Disclaimer

| :warning: | This project is no longer being maintained.
|---|---|

| Composer | [`phalette/pidic`](https://packagist.org/packages/phalette/pidic) |
|---|------------------------------------------------------------|
| Version | ![](https://badgen.net/packagist/v/phalette/pidic)      |
| PHP | ![](https://badgen.net/packagist/php/phalette/pidic)    |
| License | ![](https://badgen.net/github/license/contributte/pidic)   |

## About

PiDiC is an adapter over [Nette\Di\Container](https://api.nette.org/2.3/Nette.DI.Container.html).

## Install

```sh
$ composer require phalette/pidic:dev-master
```

### Dependencies

* **PHP >= 5.5.0**
* [Nette\Di >= 2.3.0](https://github.com/nette/di)
* [Phalcon >= 2.0.0](https://github.com/phalcon/cphalcon/)

## Configuration

```php
use Nette\DI\Compiler;
use Phalette\Pidic\Configurator;
use Phalette\Pidic\Environment;
use Phalette\Pidic\Extensions\PhalconDefaultsExtension;
use Phalette\Pidic\Extensions\PhalconExtension;
use Phalette\Pidic\PiDi;

$configurator = new Configurator();
$configurator->setMode(Environment::DEVELOPMENT);
$configurator->setCacheDir(__DIR__ . '/cache');
$configurator->onCompile[] = function (Compiler $compiler) {
    $compiler->addExtension('phalcon', new PhalconExtension());
    $compiler->addExtension('phalconDefaults', new PhalconDefaultsExtension());
};

$container = $configurator->createContainer();
$pidi = $container->getService('pidi');
```

### Learn by working example

This is based on [official tutorial](https://docs.phalconphp.com/en/latest/reference/tutorial.html).
```php
use Nette\DI\Compiler;
use Phalette\Pidic\Configurator;
use Phalette\Pidic\Environment;
use Phalette\Pidic\Extensions\PhalconDefaultsExtension;
use Phalette\Pidic\Extensions\PhalconExtension;
use Phalette\Pidic\PiDi;

use Phalcon\Loader;
use Phalcon\Mvc\View;
use Phalcon\Mvc\Application;
use Phalcon\DI\FactoryDefault;
use Phalcon\Mvc\Url as UrlProvider;
use Phalcon\Db\Adapter\Pdo\Mysql as DbAdapter;

try {

    // Register an autoloader
    $loader = new Loader();
    $loader->registerDirs(array(
        '../app/controllers/',
        '../app/models/'
    ))->register();

    // Create a DI
    $configurator = new Configurator();
    $configurator->setMode(Environment::DEVELOPMENT);
    $configurator->setCacheDir(__DIR__ . '/cache');
    $configurator->onCompile[] = function (Compiler $compiler) {
        $compiler->addExtension('phalcon', new PhalconExtension());
        $compiler->addExtension('phalconDefaults', new PhalconDefaultsExtension());
    };
    $container = $configurator->createContainer();
    $di = $container->getService('pidi');

    // Setup the view component
    $di->set('view', function () {
        $view = new View();
        $view->setViewsDir('../app/views/');
        return $view;
    });

    // Setup a base URI so that all generated URIs include the "tutorial" folder
    $di->set('url', function () {
        $url = new UrlProvider();
        $url->setBaseUri('/tutorial/');
        return $url;
    });

    // Handle the request
    $application = new Application($di);

    echo $application->handle()->getContent();

} catch (\Exception $e) {
     echo "PhalconException: ", $e->getMessage();
}
```

### PhalconExtension

It sets self-instance over static `Phalcon\Di::setDefault()`. Every object extending from `Phalcon\Di\InjectionAwareInterface` can access **PiDiC** from `$this->getDI()`.

### PhalconDefaultsExtension

This extension replace `Phalcon\DI\FactoryDefault`. It register to the container 22 base services ([more in docs](https://docs.phalconphp.com/en/latest/reference/di.html#service-name-conventions)).

## Phalcon\Di

**PiDiC** implements [Phalcon\DiInterface](https://docs.phalconphp.com/en/latest/api/Phalcon_DI.html) and then you can change DI without any changes.

How to work with DI in Phalcon, you can [read here](https://docs.phalconphp.com/en/latest/reference/di.html).

## Nette\DI

Please read articles at Nette documentation:

* [Configuration](https://doc.nette.org/en/2.3/configuring)
* [Dependency Injection](https://doc.nette.org/en/2.3/dependency-injection)
* [Di usage](https://doc.nette.org/en/2.3/di-usage)

But the main article is:

* [Extensions](https://doc.nette.org/en/2.3/di-extensions)

## Development

This package was maintained by these authors.

<a href="https://github.com/f3l1x">
  <img width="80" height="80" src="https://avatars2.githubusercontent.com/u/538058?v=3&s=80">
</a>

-----

Consider to [support](https://contributte.org/partners.html) **contributte** development team.
Also thank you for using this package.
