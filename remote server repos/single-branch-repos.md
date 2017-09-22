# Creating Single Branch Git Repos 

When working in a staging or production environment that is **not** Flywheel, it may be useful to create a remote git repo to easily push your changes without using FTP/SFTP clients. This is our preferred way of pushing code changes to servers when we can. (Some servers we can use this method on are servers like factor1.me, ther29.com, or any client hosted on Skywalker - but are not limited to these servers).

>Note: for an ongoing client where staging and production live on the same server, it may be useful to use a multiple branch strategy. See the docs on this strategy [here](https://github.com/factor1/developer-resources/blob/master/remote%20server%20repos/multi-branch-repos.md).

## What's A Single Branch Git Repo?
By single branch git repo, we mean that the `master` branch of the remote repo servers up the files that are pushed. There would never be multiple branches at the remote repo and you would always push to the `master` branch of the remote repo. 

## Setting Up The Remote Repo
> In this example, we will be setting up a new staging environment on ther29.com under a fake project called StoutRules.

The first thing we'd want to do is connect to the server via ssh in your terminal.

>Note: this example server has a custom port for SFTP/SSH, 2222 instead of 22

```sh
ssh ther29@ther29.com -p 2222
```

Then at the server root level, we will create a folder called `repos` if it doesn't already exist. (Some existing servers may also have a folder called `repo` these are interchangeable)

```sh
mkdir repos
```

Now we are ready to setup the remote repo.

### Create and initiate the repo
```sh
## create the repo folder
mkdir stoutrules.git 

## navigate to the repo
cd stoutrules.git

## initiate a bare git repo
git init --bare
```

Now we have a bare repo that we can add our hook to.

### Add a hook to handle our pushed code 
A git hook allows us to tell git to do something when it is pushed to. Here we will create a hook called a `post-receive` hook, that will do something _after_ it gets the new code changes.

```sh
## navigate to the hooks folder 
cd hooks

## create a post-receive
cat > post-receive
```

When running the `cat` command, you will automaticcaly be entered into the `post-receive` file and you can add your content. To save and exit this prompt press `CTRL+D`. (You may have to do this twice)

Now we are ready to enter the bin script that handles the file move.

### `post-receive` bin script
Below is what our bin script would look like in our project. (You can find an example bin script in GistBox, although GistBox will be removed from our systems by December 2017 and we are searching for a replacement system)

```sh
#!/bin/sh
git --work-tree=/home/ther29/public_html/wp-content/themes/stoutRules --git-dir=/home/ther29/repos/stoutrules.git checkout -f
```

Now lets break this down a little bit. `#!/bin/sh` tells the server that this is a bin script. You must include this line.

`--work-tree=` is _where_ we will want our code changes to end up, this should be a theme folder or in some cases the `public_html` folder depending on where you want your files.

`--git-dir=` is where the actual git repo lives on the server. This should be the path to where you setup the initial git repo earlier in this documentation. 

### Setting Permissions
The last step of this process is to ensure we have the proper permissions set on our script so that git can excute it. We use the [`chmod`](https://en.wikipedia.org/wiki/Chmod) command with an argument of `+x` to make it executeable. 

```sh
chmod +x post-receive
```

Now we are all set with a remote git repo we can push our changes to. 

## Setting Up Your Local Repo to Push to the Remote Repo
Now, we need to tell our local git repo that this new remote repo exists so that we can push to it.

### Adding the remote repo
You can name your remote repo whatever you like, however as a best practice we use `staging` for staging servers and `production` for production servers. We will need the path of the remote repo for this next step, so be aware of where you set this up.

From terminal in your local repo:

>Reminder: this paticular server we are working with has a port of 2222 instead of 22, so we need to add that to our remote repo url.

```sh
git remote add staging ssh://ther29@ther29.com:222/home/ther29/repo/stoutrules.git
```

Now you can verify that it successfully added by checking your remotes.

```sh
git remote -v
```

### Pushing to the Remote Repo
With the single branch strategy, we will always push to the `master` branch on the remote. You can push any local branch you want, so we just need to tell git what to push.

So lets say we want to push our changes on our local `develop` branch to staging.

```sh
git push staging develop:master
```

This should successfully push our changes to the staging server. Lets again breakdown what we are doing here, we will add some placeholders so you know exactly whats happening.

```sh
git push {remote name} {local branch}:{remote branch}
```

Again, the `{remote branch}` should always be `master` in the single branch strategy. 

## Misc. Info
Cool, so we are able to push to a remote repo and leave FTP and SFTP in the past. In some cases you may receive fast-forward errors and your pushes may be rejected. Normally when we are working with git repos this is a big problem but when pushing to the server it may happen a little more frequent and thats okay. As long as you know _your local branch_ is right - you can force push your changes.

Example:
```sh
git push staging develop:master --force
```
