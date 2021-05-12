## change version php

You can change php version of composer without uninstalling it, follow these steps :

Search for system environment variables in cortana.
Click on the button "Environment variables".
Under "System variables" select path and click on edit, you will see one entry like this "C:\wamp\bin\php\php5.6.13".
Just change this to the folder name of the php located at your wamp/bin/php7.1.9, here bin7.1.9 is folder name.
Replace php5.6.13 with bin7.1.9, it will look like these "C:\wamp\bin\php\php7.1.9", just click ok on all the boxes.
You are done.
To verify, first close all the cmd windows, than open cmd and type "php -v", press enter and you should see php7.1.9.
If you don't see change in php version than just restart your pc and run php -v again in cmd , it will work.