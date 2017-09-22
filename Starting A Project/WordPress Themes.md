# Getting Started With A WordPress Theme
At factor1, our bread and butter is custom WordPress themes. We don't buy themes and modify them, which is what makes our work stand out. This doc will help you get started on your next custom factor1 WordPress theme.

## 1. Gather Assets
Before you start, be sure that you have all the design files, template tables, and other documentation you may need to get started. Be sure that the design files include all the required WordPress views like a 404 Page, Index View, Single Post View, and default page. If you are missing any of these things be sure to reach out to Matt or the design team immediately so that we can get these issues addressed ASAP. The ultimate goal is that once a project lands in the developers hands, there are as little unknowns as possible.

## 2. Setup Your Local Environment 
For the most part we use [Local by Flywheel](https://local.getflywheel.com/) for our local dev environments. (We won't go into detail here with setting up Local since it is pretty straight forward and to avoid any outdated information. View their docs for more info on getting started with Local) 

Another application you may use from time to time is [MAMP](https://www.mamp.info/en/). Unlike Local, MAMP does not include WordPress so you will have to install it manually. MAMP is a good solution if your WordPress theme may have extra PHP scripts or libraries that need to run outside of a sandboxed WordPress environment. (Again, we will skip setup information here in favor of their docs)

For Windows Devs, [WAMP](http://www.wampserver.com/en/) is a good solution as well and is very similar to MAMP.

## 3. Setup Your Theme Path 
Now that we have our local environment setup and WordPress installed, we need to create a folder for our theme to live. Navigate to `wp-content/themes/` and create a new folder with your theme name.

## 4. Initialize a Git Repo & Create a GitHub Repo
Now that we have the path our theme will be located in, lets initiate a git repo so that we can track our changes. Via terminal in our theme directory:

```sh
git init
```

Once this has completed add a Readme so that we have a file to commit (see our Example Readme that should be used [here](https://github.com/factor1/readme-template)) and commit that file.

After our commit we can create the repo on GitHub so that we can push to it and give our theme a home outside of our local machine. It's a best practice to match the theme folder with the name of the GitHub repo as well. Once you create the repo, follow the onscreen instructions from GitHub to add the remote origin and push. 

## 5. Initialize your theme w/ npm 
Now, lets initialize our project with npm so that we can install packages such as Prelude.

```sh
npm init
```
npm will then ask you a series of questions, fill them out (some will be pre-populated such as git url). For `version` always start with `0.0.1` and for license enter in `GPL-3.0`. `Entry point`, and `test command` can be left blank. `Author` should follow this pattern: `Your Name <factor1studios.com>`.

## 6. Install Prelude & Other npm Packages
[Prelude](https://github.com/factor1/prelude-wp) is the backbone of all our custom WordPress themes. This should be used for **all** our themes unless there is a very strong case against it. To install:

```sh
npm install prelude-wp --save
```

For more information on installing and using [Prelude, view the docs here](http://prelude.factor1.org).

-------

You may now also install any other packages from npm that you may be needing such as Font Awesome, Slick Slider, etc... Always remember to use the `--save` flag so that anyone who may come into your project at a later date gets all the dependencies they need to successfully develop in your theme. 

## Developing Your Theme 
Now it's time to get down to brass tax and crank out your theme. From our experience it's best to follow this pattern when deving out your theme:

- Code the styleguide/core styles (typography, forms, etc)
- Code Header
- Code Footer
- Code Home
- So on and so forth
