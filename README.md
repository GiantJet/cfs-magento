# CFS Theme

## General Global Dependancies

For our local environment, we are using MAMP Pro to create our local server environment and use the following:

php (v5.6.30 to v7.0.15 via mamp)
composer (v1.4.2) (https://getcomposer.org/doc/00-intro.md)
node (v6.9.1)
npm (v5.0.0)
grunt (v1.2.0)

** these are the versions i'm using... others may work
** it may be helpful to up the memory limit to 1028M or more:
https://getcomposer.org/doc/articles/troubleshooting.md#memory-limit-errors

### Composer:
I used homebrew to install:
```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install composer
```

### Node & npm
I used homebrew to install:
```
brew install node
node -v
npm -v
```

### Grunt
```
npm install -g grunt-cli
grunt -version
```

## Magento Install
### Magento Code Composer Setup
We are using the composer version of magento as a starting point.

Start by Cloneing this repository

Go to the base magento folder and remove `.sample` off of grunt.js, package.json

In terminal, navigate to the base magneto folder
```
cd /{project location}/magento/
composer install
```

(if you get an mcrypt error… install it locally or point your computers php to your mamp version of php which has it installed and turned on by default… See below for instructions on Switching PHP to MAMP Version)

As it installs, it will as you for a username and password when installing some magento components. To do this, you need to create a magento account here:
https://account.magento.com/customer/account/create/

After creating an account, you need to create Marketplace Access keys.
Follow the instruction to create a public and privare key:
http://devdocs.magento.com/guides/v2.0/install-gde/prereq/connect-auth.html

The public key will be used as your username and the private key will be used as your password.

Then just sit back and wait for everything to be installed.

#### Switching PHP to MAMP Version
in terminal
```
which php
```
(this will tell you which php you are using)... if you wish to change to mamp's version:
```
touch ~/.bash_profile; open ~/.bash_profile
```
add the following to the bottom of the page (replace the php version with the one you are using):
```
export MAMP_PHP=/Applications/MAMP/bin/php/php7.0.15/bin
export MAMP_BINS=/Applications/MAMP/Library/bin
export USERBINS=~/bins
export PATH="$USERBINS:$MAMP_PHP:$MAMP_BINS:$PATH"
```
save and exit
```
source ~/.bash_profile
which php
```
it should show “/Applications/MAMP/bin/php/php7.0.15/bin/php”

And there you have it!

This info is from : https://stackoverflow.com/a/26549723

### Magento Install

create a local database. Be sure to remember the db name and user and password

Open your browser and navigate to the site (for me it’s http://localhost/cfs/magneto)

Follow the install instructions, inputing the db information.

...(Optional)
...Once that is done, login to the admin. (The first time you do this, it might take a while since it's building the cache)
...Navigate to `System -> Cache management`
...Disable the following for ease of development:
...* Configuration
...* Layouts
...* Blocks HTML output
...* Page Cache

Next, open terminal to set the build to developer mode:
```
cd /(magento directory)
php bin/magento deploy:mode:set developer
```

Next we will install all of the dependancies:
```
npm install
npm update

grunt clean
grunt exec
grunt less
```

## CFS Template Setup

Add the following folders:
app -> design -> frontend -> solve -> cfsbinds

copy the theme into that folder

Login and update the theme by navigating to `content -> configuration (under design)`
Edit the theme on the bottom of the list
Change the default theme to CFS Binds

Next we will add the theme to the theme.js. Edit the following file: `dev/tools/grunt/configs/themes.js`
add to the bottom:
```
,
    cfsbinds: {
        area: 'frontend',
        name: 'solve/cfsbinds',
        locale: 'en_US',
        files: [
            'css/styles-m',
            'css/styles-l'
        ],
        dsl: 'less'
    }
```

Finally, in terminal we will grunt:
```
cd /(magento directory)
php bin/magento cache:clean
grunt clean
grunt exec:cfsbinds
grunt less:cfsbinds
```

(helpful: https://firebearstudio.com/blog/magento-2-grunt.html)

## Extentions and Plugins

### Amasty Extensions/Plugins
We use the following Amasty Extensions/Plugins:
*AbandonedCartEmailforMagento2-1.0.6-CE
*ImprovedLayeredNavigationforMagento2-1.14.12-CE
*LoyaltyProgramforMagento2-1.2.2-CE
*ProductAttachmentsforMagento2-1.2.1-CE
*ProductFeedforMagento2-1.2.0-CE

Install guide taken/modified from Amasty:

1. Unpack All of the extension ZIP files on your computer.
2. Add all the files and folders from the extension packages to the corresponding root `app` folder of your Magento installation.
*** Please “Merge”, Do not replace the whole folders, but merge them. If you install several extensions from Amasty, they will contain same files from the Base package — feel free to overwrite them, these are system files used by all our extensions.
3. Run 3 following commands:
```
php bin/magento module:enable Amasty_Base Amasty_Acart Amasty_Shopby Amasty_ShopbyBrand Amasty_ShopbyPage Amasty_ShopbyRoot Amasty_ShopbySeo Amasty_Rules Amasty_RulesLoyalty Amasty_RulesPro Amasty_ProductAttachment Amasty_Feed
php bin/magento setup:upgrade
rm pub/static/_requirejs/frontend/Magento/luma/en_US/requirejs-config.js
```
4. Login to your site and Go to `System — Cache Management` page on your Magento backend and click the `Flush Magento Cache` button. After this action, the extension is installed.

### LitExtension Extensions/Plugins
We use the following Amasty Extensions/Plugins:
*yahoostore2magento2_v2.0.0

1. Unpack the extension ZIP file on your computer.
2. Add the files and folders from the extension packages to the corresponding root `app` folder of your Magento installation.
*** Please “Merge”, Do not replace the whole folders, but merge them. If you install several extensions from Amasty, they will contain same files from the Base package — feel free to overwrite them, these are system files used by all our extensions.
3. Run 3 following commands:
```
php bin/magento module:enable LitExtension_Core LitExtension_CartImport 
php bin/magento setup:upgrade
rm pub/static/_requirejs/frontend/Magento/luma/en_US/requirejs-config.js
```
4. Login to your site and Go to `System — Cache Management` page on your Magento backend and click the `Flush Magento Cache` button. After this action, the extension is installed.

