# Deploying A WordPress Site To Flywheel 
Flywheel is a dedicated WordPress host that we use on a bulk of our sites. The only downside to Flywheel is that we do not have SSH access, so we cannot just push our theme changes via git from the command line. Since Flywheel is a dedicated WordPress host, they have a super simple 1-click install of WordPress which can speed up the launch/staging process.

## Setting Up WordPress
**1. Login at [app.getflywheel.com](http://app.getflywheel.com)**

---

**2. Create a new Flywheel site**
   ![Flywheel Screenshot](https://github.com/factor1/developer-resources/raw/master/flywheel/flywheel-newsite.png)
   ![Flywheel Screenshot 2](https://github.com/factor1/developer-resources/raw/master/flywheel/flywheelsetup.png)
   
> Be sure to select Factor1 as the site owner and "Add to Bulk Plan" as the billing option.

Normally we would make a `.zip` file of our `uploads/` and `plugins/` but since we cannot unzip on Flywheel, we will have to either **A)** Manually install each plugin or **B)** Upload the `plugins/` folder unzipped (will take some time). You should also have the Migrate WPDB plugin installed on your local WordPress theme with the Media Files addon, since we will be using that to push our database and media files in a later step. 

---

**3. SFTP In, Upload Your Theme**

Now we can use an FTP program like Cyberduck or Transmit to upload our theme. You can find your Flywheel SFTP credentials in your account profile:

![Flywheel Profile Screenshot](https://github.com/factor1/developer-resources/raw/master/flywheel/profile.png)

Make sure your SFTP info is correct. Once you're logged in, navigate to the site you have just created and upload your theme to the `/themes/` folder on Flywheel, then if you're uploading the `plugins/` folder instead of manually installing plugins you should do this now as well. 

---

**4. Moving the Database and Media Files**

Now we can get started moving the database from your local install to the server, as well as the media files. Login to your new WordPress install on the Flywheel server, and upload the Migrate WPDB plugin if its not already there, and activate the Media Files addon as well. 

Now we can go ahead and use Migrate WPDB Pro to move our database and media files over to our fresh install on Flywheel, and you're ready to rock! 🤘 

#### A Note About Database Prefixes 
By default, Flywheel gives your database a unique prefix that may not match up to what your local prefix is using. If thats the case, you can edit the Flywheel DB prefix via the Flywheel admin panel or match your local db prefix to what Flywheel is using. (Flywheel does not allow the editing of `wp-config.php` - so keep this in mind)

[Click here for docs on using WP Migrate DB Pro](https://github.com/factor1/developer-resources/blob/master/WP%20Migrate%20DB/Syncing%20Databases.md)

> **HELP!** If the media file transfer fails, try again. If it fails again... bummer - but you'll have to just manually upload the `/uploads/` folder to Flywheel. 
