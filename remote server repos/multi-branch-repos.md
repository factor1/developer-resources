# Remote Repos With Multiple Branches 
In some cases, we may have a production site and a staging site that both live on the same server and we may be working on both simultaneously. We can use a multiple branch system on the remote repo so that when we push changes to the remote on `master` it goes live, and when we push to `develop` on the remote, it appears on staging. (Good client examples that can benefit from this -but might not actually use this: Tathata Golf, Lifewater, Rule29.com, our own site)

(There is also a good post on [Medium](https://medium.com/factor1/setting-up-server-environments-for-a-seamless-git-deployment-d0b88e8d1c24) on this by our Senior Dev - Eric, which most of the content on this page comes from)

## Setting Up The Remote Repository
Login to your server via your terminal and lets create a folder for your repo to live in.

```sh
## Login to your server
$ ssh youruser@yourdomain.com
## create repo folder 
$ mkdir repos
## navigate to created folder 
$ cd !$
```

Awesome. Now lets make an empty git repository inside of `/repos/`.

```sh
## create and name your repo folder
$ mkdir yourrepo.git
## navigate to your repo folder
$ cd !$
## initialize empty repo
$ git init --bare
```

Now here is where the fun comes in. We’re going to need to setup whats called a `post-receive` hook to tell our repo what to do with the branches once they are received. So lets navigate to our hooks folder. Chances are, there is not a post-receive file so we will have to create one.

```sh
## navigate to hooks folder
$ cd hooks
## create post-receive file 
$ touch post-receive 
```

Now we’re ready to go… Let’s dive into the script we will be using to do the magic. The first thing we need to do in our script is tell it where our staging and production trees will be living. We are going to assume that our production lives in `public_html/` and that staging lives in `public_html/staging/` to make it nice and easy. Of course, you can have your environments live where ever you want on your server. Open `post-receive` using your editor of choice and lets get this rolling.

```sh
#!/bin/sh

GIT_DIR=$PWD
STAGING_TREE=/home/youruser/public_html/staging
PRODUCTION_TREE=/home/youruser/public_html/
GIT_WORK_TREE=
```

The above section is pretty straight forward. We are telling the script that the git directory, `GIT_DIR` lives in the previous working directory, and that the staging and production trees live in their respective spots on our server. Next we need to setup our git work tree, so that the repository knows where to place the files based on branches. So continuing in the same file:

```sh
while read oldrev newrev refname
do
  	GIT_BRANCH=$(git rev-parse --symbolic --abbrev-ref $refname)
        if (test "$GIT_BRANCH" = "master" ); then
                cd $PRODUCTION_TREE || EXIT
                GIT_WORK_TREE=$PWD
        fi
	if (test "$GIT_BRANCH" = "develop" ); then
                cd $STAGING_TREE || EXIT
                GIT_WORK_TREE=$PWD
        fi
done
```

What this section of the script does is check the commits and decide where to place them. If the commits exists on `master` then it puts it in the production tree, if its in the branch `develop` it moves it to the staging tree.
Next we just need to add one more piece to manage our work trees and branches. Continuing in the same file add:

```sh
if (test "$GIT_WORK_TREE" != ""); then

        cd $GIT_DIR
        git --work-tree="$GIT_WORK_TREE" checkout $GIT_BRANCH -f

fi
```

This just makes sure that the correct branch and work trees are checked out.
All together, your script should look something like this:

```sh
#!/bin/sh

GIT_DIR=$PWD
STAGING_TREE=/home/youruser/public_html/staging/
PRODUCTION_TREE=/home/youruser/public_html/
GIT_WORK_TREE=

while read oldrev newrev refname
do
  	GIT_BRANCH=$(git rev-parse --symbolic --abbrev-ref $refname)
        if (test "$GIT_BRANCH" = "master" ); then
                cd $PRODUCTION_TREE || EXIT
                GIT_WORK_TREE=$PWD
        fi
	if (test "$GIT_BRANCH" = "develop" ); then
                cd $STAGING_TREE || EXIT
                GIT_WORK_TREE=$PWD
        fi
done

if (test "$GIT_WORK_TREE" != ""); then

        cd $GIT_DIR
        git --work-tree="$GIT_WORK_TREE" checkout $GIT_BRANCH -f

fi
```

One last step we need to do on our server is set the right permissions for git to be able to execute the script. So in the `hooks/` directory simply run `chmod +x post-receive` and we’re set.

## Setting Up Your Local Repository
Now we just need to setup your local repository to have our new server side repository as a remote. Again, we’re going to assume you’re already up and running with a project using git. So let’s add the remote repository, we’re going to name it “server”.

```sh
$ git remote add server ssh://youruser@yourdomain.com/home/repos/yourrepo.git
```

Now, we can test our first push. Let’s push our `develop` branch to staging.

```sh
$ git push server develop:develop
```

This command then pushes our local `develop` branch to the `develop` branch on our remote server/repo which is mapped to our staging environment.

We repeat the same process for production:

```sh
$ git push server master:master
## or 
$ git push server
```

And that’s it! You did it. No need for FTP, no need for two remote repositories, your branches push to their respective environments and your dev time just got a little easier and shorter.
