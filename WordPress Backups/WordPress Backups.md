# Backing Up WordPress
Backing up your WordPress files and database is a crucial part of our development process no matter if it is a launched project, or in developement. This _will_ and _can_ save you loads of headaches in case of equipment failure or catastrophic bugs.

## Backing Up Locally 
When starting a new project and doing development locally, you should be using Git and pushing frequently, so this takes care of our theme files for us. As for the database, we need to make sure we are backing up as often as possible in case our work is destroyed or we need to roll back for one reason or another.

### BackupWordPress
[BackupWordPress](https://wordpress.org/plugins/backupwordpress/) is our go-to WordPress backup plugin and should be the **only** plugin you use for backups. BackupWordPress gives us the option to backup just the database, or do a full backup with database _and_ theme files. When working locally, there is no need to run complete backups.

#### Setting Up Daily DB Backups
Setting up the daily backups is very simple and straightforward. Just be sure you are on the "Database Daily" tab, and click settings. Ensure that "Once Daily" is selected via the schedule dropdown and you're all set.

> It's a good idea to set the amount of backups to 14, to ensure you have a good history of your database while in development

#### Pushing DB Backups to Rackspace 
Having our database backup only to our machine is going to do us no good if your machine fails so we connect BackupWordPress to Rackspace to have them saved offsite.

1. Install and activate the plugin BackUpWordPress To Rackspace Cloud which can be found in our shared WordPress Resources folder in Dropbox
2. Navigate to Backups via the WP Admin and click "Rackspace Cloud"
3. Check "Send a copy of each backup to Rackspace Cloud"
4. Enter the Rackspace details (they can be found in our shared 1password under "rackspace.com"
5. Set the max backups to 14.

> Be sure to **not** push complete backups to Rackspace

## Backing Up In Production
When a site goes live we want to make sure we are still maintaining a good process for backups. While our servers do complete backups, they can be slow and difficult to recover. Where the site will be living will dictate how we backup.

### Backing Up Sites On Flywheel
Flywheel automatically backup your database so there is no action needed. It will also automatically deactivate any backup plugins running, so don't bother migrating BackupWordPress to Flywheel sites.

### Backing Up Sites On Everything Else 
1. Follow the same steps for DB backups as local, but reduce the **max backups to 7**.
2. Click Complete Weekly Tab
3. Click Settings 
4. Ensure "Schedule" is set to "Once Weekly"
5. Set "Number of backups to store on this server" to 4 and click done.
6. Click "Rackspace Cloud"
7. Ensure "send a copy of each backup to Rackspace Cloud" is checked
8. Enter Rackspace Cloud details if they aren't already populated
9. Set "Max Backups" to 4 and click "Done"
