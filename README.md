# Magento2 Default Installation Error Fixes

Providing default .htaccess files to fix 500 errors when clean installations of Magento2 just don't work. 

## Composer Installation
1) Connect with SSH (Mac/Linux: Use Terminal; Windows use PuTTy) 
  * Mac) `ssh -p {{port}} {{user}}@{{ip}}`
  * Windows) GUI Makes the fields simple. 
Install Composer in the root directory (`ls` should show `'public_html'`) `wget https://getcomposer.org/installer`
    `ls` (to make sure installer has been downloaded)
  2.2) `php installer —check`
  2.3) Make sure no errors pop up here…. They shouldn’t
  2.4) `php installer`
  2.5) `rm -f installer`
3) Composer is now installed

## Magento 2.3.4 Installation
_Make sure your PHP version is compatible with the current Magento release._

1) Make an account on the Magento Marketplace, create authentication keys here. https://marketplace.magento.com/ (Profile -> My Authentication keys)
2) Your public key will be your username
Your private key will be your password.
Go back to your cPanel window. Navigate to /public_html/
Make a new file for your Magento2 installation, I called mine magento2
Back in terminal - cd magento2
Install Magento
	composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition .
	(The ‘.’ Is important, it represents the directory that you’re currently in). 
	find var generated vendor pub/static pub/media app/etc -type f -exec chmod u+w {} + && find var generated vendor pub/static pub/media app/etc -type d -exec chmod u+w {} + && chmod u+x bin/magento
	 php bin/magento setup:static-content:deploy -f
	 php bin/magento indexer:reindex
	 php bin/magento cache:clean
	 php bin/magento cache:flush
Back on cPanel, open File Manager
Click on ‘settings’ and, ’Show Hidden Files (dotfiles)’.
In the root folder (/public_html/magento2/), change the contents of the .htaccess file to code-snippet(1) 
Navigate to /public_html/magento2/pub/ and change the contents of the .htaccess file to code-snippet(2)
Navigate to /public_html/magento2/pub/static and change the contents of the .htaccess file to code-snippet(3)
Go to {{yourdevsite}}.elephantdev.co.uk/magento2
Run through the installation process, change your homeURL to {{yourdevsite}}.elephantdev.co.uk/magento2 (magento2 is crucial, this is your installation dir, if you didn’t set it to magento2 earlier use whatever you called it. You can change this in the database if you mess it up). 
Check both your homeURL and your adminURL to make sure both are working. If not, check the htaccess files that you edited before.
