# Git Best Practices 

## Commits 
A commit message should be clear and descriptive of what changed in your code. An example of a poor commit would be:

> Updated some css

A better commit message:

> Update header padding and link colors in site navigation

## Branch Management 
Understanding where you should be doing your work is key to our success as a development team. 

### Master Branch
The `master` branch should be the released version of your code/project. This would be the code that is currently living on the live production server, plugin repository, or npm package. **You should never commit code directly on `master`**, code should always be merged into `master`. The only small exception would be a simple version bump to the project.

### Development Branch
The `development` branch represents what is being worked on or what is live on the staging server. This code will eventually be merged into `master` and go live. While you _can_ commit code directly on to develop (which you may do at the beginning stages of a project) it is **highly recommended to work on feature branches**.

### Feature Branches 
A feature branch is a branch that is dedicated to the development of a specific feature, such as a header navigation. In this case, we would create a new branch called `feature/header-navigation` and once we are done developing this feature we would merge it back into `develop`. 

### Hotfix Branches 
You will often come across a quick fix, also known as a "hot fix." Generally a hotfix is something that we need to fix asap, as it affects the live server. A hotfix branch should always be pulled from `master` and merged back into `master`. Lets look at an example where we would be changing link colors.

```sh
# on branch master
git checkout -b hotfix/link-colors

git commit -am "Update link colors"

# checkout master again, pull for any changes, merge, then push to GitHub
git checkout master && git pull && git merge --no-ff hotfix/link-colors && git push
```

Now we have successfully done a hotfix. 

### Release Branches 
We rarely work with release branches when it comes to our day to day processes working on custom WordPress themes. We use them on a case by case basis when working with open source projects such as Prelude, or plugin code bases such as Better Rest Endpoints.

## Merging 
When merging, always be sure to use "no flash forward."
`git merge -no--ff develop`

## Tagging Releases 
When you have finished a release or update and you've updated the version of your theme/codebase be sure to tag it in Github as well. 

```sh
# show all tags 
git tag

# tag a version 
git tag 1.0.1

# push tags 
git push --tags
```

>When it comes to WordPress theme development, any time you update the theme version using the Pelude gulp task, you should add a new Git tag as well. 

## More Resources
For more resources, view the [Git Docs](https://git-scm.com/docs)
