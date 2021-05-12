## Change version php

IMO there is no need to switch the environment variables just for running composer commands with a different PHP Version (which were not set in your env paths.). Just run your composer.phar with a absolut PHP execution file path and you will be fine. Command pattern: $ <pathToPhPExecutionFile> <pathToComposerPhar> install.

This should do it if you have composer installed globally:

On windows e.g.: C:\xampp\php\php.exe C:\ProgramData\ComposerSetup\bin\composer.phar install
On *ngix run e.g.: /usr/bin/php /usr/local/bin/composer install
