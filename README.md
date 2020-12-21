# Backpack\BackupManager

[![Latest Version on Packagist](https://img.shields.io/packagist/v/backpack/backupmanager.svg?style=flat-square)](https://packagist.org/packages/backpack/backupmanager)
[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE.md)
[![Build Status](https://img.shields.io/travis/laravel-backpack/backupmanager/master.svg?style=flat-square)](https://travis-ci.org/laravel-backpack/backupmanager)
[![Coverage Status](https://img.shields.io/scrutinizer/coverage/g/laravel-backpack/backupmanager.svg?style=flat-square)](https://scrutinizer-ci.com/g/laravel-backpack/backupmanager/code-structure)
[![Quality Score](https://img.shields.io/scrutinizer/g/laravel-backpack/backupmanager.svg?style=flat-square)](https://scrutinizer-ci.com/g/laravel-backpack/backupmanager)
[![Style CI](https://styleci.io/repos/53956594/shield)](https://styleci.io/repos/53956594)
[![Total Downloads](https://img.shields.io/packagist/dt/backpack/backupmanager.svg?style=flat-square)](https://packagist.org/packages/backpack/backupmanager)

An admin interface for [spatie/laravel-backup](https://github.com/spatie/laravel-backup). Allows the admin to easily manage backups (download and delete). Used in the Backpack package, on Laravel 5.2+ to 6.



> ### Security updates and breaking changes
> Please **[subscribe to the Backpack Newsletter](http://backpackforlaravel.com/newsletter)** so you can find out about any security updates, breaking changes or major features. We send an email every 1-2 months.
![Backpack\BackupManager screenshot](https://backpackforlaravel.com/uploads/screenshots/backups_running.png)


## Install

1) In your terminal

``` bash
# Install the package
composer require backpack/backupmanager

# Publish the config file and lang files:
php artisan vendor:publish --provider="Backpack\BackupManager\BackupManagerServiceProvider"  --tag=config

# [optional] Add a sidebar_content item for it
php artisan backpack:add-sidebar-content "<li class='nav-item'><a class='nav-link' href='{{ backpack_url('backup') }}'><i class='nav-icon la la-hdd-o'></i> Backups</a></li>"
```

2) Add a new "disk" to config/filesystems.php:

```php
        // used for Backpack/BackupManager
        'backups' => [
            'driver' => 'local',
            'root'   => storage_path('backups'), // that's where your backups are stored by default: storage/backups
        ],
```
This is where you choose a different driver if you want your backups to be stored somewhere else (S3, Dropbox, Google Drive, Box, etc).

3) [optional] Modify your backup options in ```config/backup.php```, then run ```php artisan backup:run``` to make sure it's still working.

4) [optional] Instruct Laravel to run the backups automatically in your console kernel:

```php
// app/Console/Kernel.php

protected function schedule(Schedule $schedule)
{
   $schedule->command('backup:clean')->daily()->at('04:00');
   $schedule->command('backup:run')->daily()->at('05:00');
}
```

5) Check that it works

If the "unknown error" yellow bubble is thrown and you see the "_Backup failed because The dump process failed with exitcode 127 : Command not found._" error in the log file, either mysqldump / pg_dump is not installed or you need to specify its location.

You can do that in your config/database.php file, where you define your database credentials, by adding the dump variables. Example for Mac OS X's MAMP:

```php
'mysql' => [
            'driver'            => 'mysql',
            'host'              => env('DB_HOST', 'localhost'),
            'database'          => env('DB_DATABASE', 'forge'),
            'username'          => env('DB_USERNAME', 'forge'),
            'password'          => env('DB_PASSWORD', ''),
            'charset'           => 'utf8',
            'collation'         => 'utf8_unicode_ci',
            'prefix'            => '',
            'strict'            => false,
            'engine'            => null,
            'dump' => [
               'dump_binary_path' => '/Applications/MAMP/Library/bin/', // only the path, so without `mysqldump` or `pg_dump`; this is a working example from MAMP on Mac OS
               'use_single_transaction',
               'timeout' => 60 * 5, // 5 minute timeout
               // 'exclude_tables' => ['table1', 'table2'],
               // 'add_extra_option' => '--optionname=optionvalue',
            ]
        ],
```

## Usage

Point and click, baby. Point and click.

Try at **your-project-domain/admin/backup**


## Upgrading from 2.x to 3.x

Change your required version to ```"backpack/backupmanager": "^3.0",``` and run ```composer update```. There are no breaking changes just icons that are show using ```la la-icon``` instead of ```fa fa-icon```.


## Upgrading from 1.2.x to 1.3.x

1) change your required version to ```"backpack/backupmanager": "^1.3",``` and run ```composer update```;
2) delete the old config file (too many changes, including namechange): ```rm config/laravel-backup.php```
3) republish the config files: ```php artisan vendor:publish --provider="Backpack\BackupManager\BackupManagerServiceProvider"```
4) change your db configuration in ```config/database.php``` to use the new dump configuration (all options in one array; the example below is for MAMP on MacOS):

```
            'dump' => [
               'dump_binary_path' => '/Applications/MAMP/Library/bin/', // only the path, so without `mysqldump` or `pg_dump`
               'use_single_transaction',
               'timeout' => 60 * 5, // 5 minute timeout
               // 'exclude_tables' => ['table1', 'table2'],
               // 'add_extra_option' => '--optionname=optionvalue',
            ]
```
5) Create a backup in the interface to test it works. If it doesn't try ```php artisan backup:run``` to see what the problem is.


## Upgrading from 1.1.x to 1.2.x

1) change your required version to ```"backpack/backupmanager": "^1.2",```;
2) the only breaking change is that the ```config/database.php``` dump variables are now inside an array. Please see the step 8 above, copy-paste the ```dump``` array from there and customize;


## Change log

Please see [CHANGELOG](CHANGELOG.md) for more information what has changed recently.

## Testing

``` bash
$ composer test
```

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

## Overwriting Functionality

If you need to modify how this works in a project: 
- create a ```routes/backpack/backupmanager.php``` file; the package will see that, and load _your_ routes file, instead of the one in the package; 
- create controllers/models that extend the ones in the package, and use those in your new routes file;
- modify anything you'd like in the new controllers/models;

## Security

If you discover any security related issues, please email hello@tabacitu.ro instead of using the issue tracker.

Please **[subscribe to the Backpack Newsletter](http://backpackforlaravel.com/newsletter)** so you can find out about any security updates, breaking changes or major features. We send an email every 1-2 months.

## Credits

- [Cristian Tabacitu](https://github.com/tabacitu)
- [All Contributors](../../contributors)

## License

Backpack is free for non-commercial use and 49 EUR/project for commercial use. Please see [License File](LICENSE.md) and [backpackforlaravel.com](https://backpackforlaravel.com/#pricing) for more information.

## Hire us

We've spend more than 10.000 hours creating, polishing and maintaining administration panels on Laravel. We've developed e-Commerce, e-Learning, ERPs, social networks, payment gateways and much more. We've worked on admin panels _so much_, that we've created one of the most popular software in its niche - just from making public what was repetitive in our projects.

If you are looking for a developer/team to help you build an admin panel on Laravel, look no further. You'll have a difficult time finding someone with more experience & enthusiasm for this. This is _what we do_. [Contact us - let's see if we can work together](https://backpackforlaravel.com/need-freelancer-or-development-team).
