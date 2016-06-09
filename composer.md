# Composer - Dependency Manager for PHP

## Composer Cheat Sheet for developers
[http://composer.json.jolicode.com/](http://composer.json.jolicode.com/)

## Install composer globally
```bash
$ php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
$ php -r "if (hash_file('SHA384', 'composer-setup.php') === '92102166af5abdb03f49ce52a40591073a7b859a86e8ff13338cf7db58a19f7844fbc0bb79b2773bf30791e935dbd938') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
$ php composer-setup.php --install-dir=/usr/local/bin --filename=composer
$ php -r "unlink('composer-setup.php');"
```
## Composer.json: Project Setup
To start using Composer in your project, all you need is a composer.json file. This file describes the dependencies of your project and may contain other metadata as well.
```json
{
    "name": "celavi/my_project",
    "description": "My New Project",
    "authors": [
        {
            "name": "Ales Loncar",
            "email": "ales@celavi.org"
        }
    ],
    "require": {
        "monolog/monolog": "1.17.*"
    }
}
```
## Installing Dependencies
To install the defined dependencies for your project, just run the install command.
```bash
$ composer install
```
## Specifying Versions

### Version Range

Using comparison operators you can grab version higher than 1.3, lower than 1.8 or follow an even more complex ruleset by using AND and OR logic. Operators used can be >, <, >=, <= and !=. AND logic is represented by a space or comma, OR logic is represented by double pipes: ||.

Specifying >2.7 would mean any version above 2.7. >2.7 <=3.5 would allow for any version above 2.7 right up until – and including – 3.5.

### Wildcard Versions

By using a wildcard you can specify a pattern. 2.3.* would be encompass everything above and including 2.3.0 and below – but not including – 2.4.0. It is equivalent to >=2.3.0 <2.4.

### Hyphen Ranges

Hyphen ranges allow you to specify a range more easily, although it is a bit easier to get confused because of how it handles partial versions. A full version consists of three numbers in which case hyphen ranges make perfect sense.

2.0.0 - 3.0.0 is an inclusive range which means that all versions above and including 2.0.0 and below and including 3.0.0 will be accepted.

Partial versions like 2.0 - 3.0 means any version above – and including – 2.0 right up until but not including version 3.1.

The reason for this seemingly weird behaviour is that the left side is inclusive, the right side is completed with a wildcard. The expression above would be equivalent to >=2.1 <3.1.0

### Tilde Range

A tilde range is great for targeting a minimum required version and allowing anything up to, but not including, the next major release. If you specify ~3.6 you are allowing 3.6 and everything up to, but not including 4.0.

This method of specifying a version is equivalent to >-3.6 <4.0.

### Caret Range

The caret range is meant to allow all non-breaking updates. If a project follows semantic versioning there shouldn’t be any enhancements that break compatibility within a major branch. That is to say any thing above and including a major version and below and not including the next major version should be safe to use. By specifying ^3.3.5 you are allowing anything right up to, but not including, 4.0.

### Dev-Master

By specifying dev-master you are grabbing the currently developed latest release which hasn’t been tagged with a version number yet. This can be just fine while developing but you need to be aware that the potential for bugs is higher in these versions.

## Locking
Locking of dependencies is one of the most useful features of Composer. I mentioned the composer.lock file eariler. What this does is lock down the versions of the used components.

The lock file can make sure that everyone works with the same versions of files. Just because the application shouldn’t break due to a component update doesn’t mean that all your teammates and your production server should all be running separate versions.

When you first use Composer to grab a dependency it writes the exact version to the lock file. If you specified 2.3.* and 2.3.5 is the latest version the installed version will be 2.3.5 and it will be entered into the lock file.

Let’s say a developer joins the team 10 days later. By this time the dependency has been updated to 2.3.6. If he uses the correct command (composer install) he will receive 2.3.5 because it is locked in place.

You can of course decide to update your dependencies. In this case you should run composer update. This will grab the latest versions allowed and write them to the lock file. This will then be distributed to all sources which can in turn be updated.

## Development Requirements
Composer allows you to specify development requirements. This is done by specifying your requirements in the require-dev array instead of the require array.
```json
{
    "name": "celavi/my_project",
    "description": "My New Project",
    "authors": [
        {
            "name": "Ales Loncar",
            "email": "celavi@ghostmail.com"
        }
    ],
    "require": {
        "monolog/monolog": "1.17.*"
    },
	"require-dev": {
        "phpunit/phpunit": "~4.5"
}
```
Be aware that development requirements are always installed by default, Composer doesn’t magically know when it is being run on your production server. If you want to exclude development requirements you will need to run the install or update command with the --no-dev option.
## Tips & Tricks
### 1. Update only one vendor
```bash
$ composer update foo/bar
```
This will only install or update the library (plus its dependencies) and overwrite the *composer.lock*. You could get:
> Warning: The lock file is not up to date with the latest changes in composer.json, you may be getting outdated dependencies, run update to update them.
Ok, so how to proceed? The update command is the one which update the lock file. But if I just add a description, I may not want to update any library. In that case use the --lock parameter.

```bash
$ composer update --lock
```
### 2. Add a library without editing your composer.json
```bash
$ composer require "foo/bar:1.0.0"
```
### 3. Easy fork
Speaking about composer.json initialization, did you ever used the create-project command?
```bash
$ composer create-project doctrine/orm path 2.2.0
```
### 4. Prefer dist packages and cache them
Did you know that dist packages are now cached in your home directory?
To force downloading archive instead of cloning sources, use the --prefer-dist option included in the install and update command.
Here is a demonstration (I use the --profile option to show the execution time)
```bash
$ composer install --prefer-dist --profile
```
### 5. Prefer source to edit your vendors
For practical reasons, you may prefer cloning sources instead of downloading packages.
```bash
$ composer update symfony/yaml --prefer-source
```
Then to see modified files in your vendor:
```bash
$ composer status -v
```
### 6. Be ready for production
Just a reminder, before deploying your code in production, don't forget to optimize the autoloader
```bash
$ composer dump-autoload --optimize --no-dev
```
