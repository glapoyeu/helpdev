## Change version php

IMO there is no need to switch the environment variables just for running composer commands with a different PHP Version (which were not set in your env paths.). Just run your composer.phar with a absolut PHP execution file path and you will be fine. Command pattern: $ <pathToPhPExecutionFile> <pathToComposerPhar> install.

This should do it if you have composer installed globally:

On windows e.g.: C:\xampp\php\php.exe C:\ProgramData\ComposerSetup\bin\composer.phar install
On *ngix run e.g.: /usr/bin/php /usr/local/bin/composer install

## SHA384 is not supported by your openssl extension, could not verify the phar file integrity
I get the error when i run composer self-update on Windows 10
Here is the full error message 

```
     composer self-update
     Updating to version 1.8.4 (stable channel).
     Downloading (100%)


     [RuntimeException]
    SHA384 is not supported by your openssl extension, could not verify the phar file integrity
```
I am running PHP 7.3.2 What could be the problem?



Remove Your older versions and install the latest version of Composer,

Remove Your older version: sudo rm -f /usr/local/bin/composer

Download the installer: sudo curl -s https://getcomposer.org/installer | php

Move the composer.phar file: sudo mv composer.phar /usr/local/bin/composer
