# Cardigram custom Heroku Multi Procfile buildpack

_tl;dr_ -- The idea here is that you have a single git repository, but multiple Heroku apps. In other words, you want to share a single git repository to power multiple Heroku apps. So, for each app you need this buildpack, and for each app, you need to set a config variable named PROCFILE to the location where the procfile is for that app, and a config variable names PACKAGE to the location of the package configuration (package.json). As an example:

```
$ heroku create -a example-1
$ heroku create -a example-2

$ heroku buildpacks:add -a example-1 https://github.com/heroku/heroku-buildpack-multi-procfile
$ heroku buildpacks:add -a example-2 https://github.com/heroku/heroku-buildpack-multi-procfile

$ heroku config:set -a example-1 PROCFILE=Procfile
$ heroku config:set -a example-1 PACKAGE=package.json

$ heroku config:set -a example-2 PROCFILE=backend/Procfile
$ heroku config:set -a example-2 PACKAGE=backend/package.json

$ git push https://git.heroku.com/example-1.git HEAD:master
$ git push https://git.heroku.com/example-2.git HEAD:master
```

When example-1 builds, it'll copy Procfile and package.json into /app/*, and when example-2 builds, it'll copy backend/* to /app/*. For example-2, the process types available for you to scale up will be the ones referenced (originally) in backend/Procfile.
