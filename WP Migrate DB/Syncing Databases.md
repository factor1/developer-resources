# Syncing WordPress Databases With WP Migrate DB Pro

Using the WP Migrate DB Pro plugin we can quickly and easily migrate databases and media files (using the Media Files Add On) from site to site. You can find the activation code in 1Password. 

>**NOTE:** If you are syncing a local WP install with a server side WP install, you will need to _push_ from the local install to the server side install - a server side WP install cannot pull from a local install.

>**NOTE:** If media files fail when transferring check to make sure that Wordfence is not activated as it will block media transfers. If it still fails, you may need to manually upload the `uploads/` folder.

**1. Install Plugin on both WordPress instances**

**2. On the install you want _replaced_ (usually the fresh install) enable Push Permissions**
![Enable Push Permissions](https://github.com/factor1/developer-resources/raw/master/WP%20Migrate%20DB/enable-push.png)


**3. Copy the connection info from the install that will be _replaced_ (same screen/location as above screenshot)** 

**4. Push database changes to the database you want _replaced_**

So now that we have done step 2 and 3 on the WP install that will be replaced, we need to go back to the WP install we are pushing (usually your local install - or staging if you're pushing to a production site) and start the push process. Select **Push** and enter the connection info for the site we will be replacing. If you are transfering Media Files as well, be sure to check that box. WP Migrate DB is smart enough to replace all our URLs for us so once we transfer the database we are all set. 

![Push Database](https://github.com/factor1/developer-resources/raw/master/WP%20Migrate%20DB/push-database.png)

> **NOTE:** Some sites may have different database prefixes. After the transfer finishes, you can edit the `wp-config.php` file to point to the new prefix. If you can't edit the `wp-config.php` file you can use a [plugin](https://wordpress.org/plugins/wp-prefix-changer/) to change it from WordPress. Also see a note about Flywheel DB prefixes [here](https://github.com/factor1/developer-resources/blob/master/flywheel/Deploying%20On%20Flywheel.md#a-note-about-database-prefixes)
