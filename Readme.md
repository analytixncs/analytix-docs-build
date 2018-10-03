# Building & Deploying Analytix-docs to Heroku and Github

## The URL to access is [https://analytix-docs.herokuapp.com/](https://analytix-docs.herokuapp.com/)

This repository contains the static website build from the **analytix-docs** docusaurus repository.

As you update the documents in the **analytix-docs** repository or modify the config files, you will want to deploy to the Heroku site so users have access to the updated docs.

To make this happen, you will first need to *build* the docusaurus **analytix-docs** site.  You can do this by going to the following directory `\analytix-docs\website\`and running the following.

```
$ yarn run build
```

This will build the static website in the `\analytix-docs\website\build\analytix-docs`directory.  This would then need to be moved to the **analytix-docs-build** repository directory.

You may do this manually or optionally, you can run the `heroku-build` command which will first build the website and then copy all the files to the *analytix-docs-build* directory.  This copy assumes that both the *analytix-docs* and *analyix-docs-build* directories are at the same level.

someRoot/
├── analytix-docs/
├── analytix-docs-build/
├── ...
└── ...

Once the build is finished, you will need to commit the changes to github.  To do this run the following, again from the *analytix-docs-build* directory:

```
$ git add .
$ git commit -m "your comment"
$ git push -u origin master
```

Lastly, the changes must be deployed to Heroku. 

First make sure you are in the *analytix-docs-build* directory.  Then whenever changes have been made, simply push to the heroku master

```
$ git push heroku master
```

## The Easy Way

I have setup an NPM script in the website's *package.json* file called **heroku-build**.  When you run this, it will use the npm-run-all module to run the *build* script and then a copy command that copies the build files to the analytix-docs-build directory.

Here is the scripts from *package.json*

```json
"heroku-build": "npm-run-all build heroku -s",
"build": "docusaurus-build",
"heroku": "xcopy /E /Y \".\\build\\analytix-docs\\*.*\" \"..\\..\\analytix-docs-build\"",
```

To run the above, just get to the  `\analytix-docs\website\` and run 

```
$ yarn run heroku-build
```

Then follow the above directions for committing changes, pushing to master and then pushing to heroku.

##  NOTE: Heroku Deploy Requirement

Since Heroku only deploys apps and not static websites, we have to make it think this site is a web app.  This is done by include a simple `index.php`file in the *build\analytix-docs* directory.  

This is not part of the docusaurus build, but has been added.  If for some reason it is deleted, simply recreate it as **index.php**:

```php
<?php header( 'Location: /index.html' ) ;  ?>
```



