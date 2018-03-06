---
title: Unknown Technology
modified_at: 2015-11-03 17:03:00
tags: app error detection
---

When pushing your app to Scalingo, you might get the following error:

```text
<-- Start deployment of [my-app] -->
 !     We didn't find the type of technology your are using...
       We're sorry this build is failing!

       Refer to the documentation to find out why:
       http://doc.scalingo.com/deployment/unknown-technology

       If you can't find the issue in your application,
       please send us an email at support@scalingo.com

       <3,
       Scalingo
 !   Error deploying the application
 !   → Invalid return code from buildpack
```

## Solutions

### Project in a subdirectory of the Git repository

If the project you want to deploy is not at the root of your Git repository,
you need to define the `PROJECT_DIR` environment variable ([see
documentation]({% post_url platform/deployment/2000-01-01-project-dir %})).

### Technology detection

In order to detect the technology used by your application, we iterate over the
technologies alphabetically. It means that if your project contains multiple
technologies, we will pick the first one detected.

If you want to skip the detection phase and force the use of a specific
buildpack, add the environment variable `BUILDPACK_NAME` to your project.

If you need to use multiple technologies you can use the [multi-buildpacks]({%
post_url platform/deployment/buildpacks/2000-01-01-multi %}).

You can also develop your own buildpack and add the environment variable
`BUILDPACK_URL` to have complete control on the detection and build phases.

More information are available on [buildpacks]({% post_url
platform/deployment/buildpacks/2000-01-01-buildpacks %}) or [multi-buildpacks]({%
post_url platform/deployment/buildpacks/2000-01-01-multi %}).

Here is how we detect your technology:

#### [Ruby]({% post_url languages/ruby/2000-01-01-start %})

A `Gemfile` should be present at the root of your repository.

#### [Node.js]({% post_url languages/nodejs/2000-01-01-start %})

The file `package.json` should be present at the root of the project.

#### [Meteor]({% post_url languages/meteorjs/2000-01-01-start %})

The directory `.meteor` should be present at the root of your project.

#### [PHP]({% post_url languages/php/2000-01-01-start %})

You need to have either an `index.php` file or both `composer.json` and `composer.lock` files at the root of your project.

#### [Python]({% post_url languages/python/2000-01-01-start %})

The file `requirements.txt` and `setup.py` should be present at the root of your project.

#### [Java]({% post_url languages/java/2000-01-01-start %})

The file `pom.xml` should be present at the root of your project.

#### [Scala]({% post_url languages/scala/2000-01-01-start %})

You need to have at least an `*.sbt` file or a `project/*.scala`/`.sbt/*.scala` file or a `project/build.properties` file.

#### [Groovy]({% post_url languages/groovy/2000-01-01-start %})

The file `grails-app` must be at the root of your project.

#### [Clojure]({% post_url languages/clojure/2000-01-01-start %})

The file `project.clj` must be at the root of your project.

#### [Go]({% post_url languages/go/2000-01-01-start %})

You need to have at least one `*.go` file at the root of your project.
Then, we detect the Go language and install any dependency with your `Godeps` directory (see more about [Godeps](https://github.com/tools/godep)).

#### [Haskell]({% post_url languages/haskell/2000-01-01-start %})

A file `*.cabal` must be at the root of your project.

#### [Erlang]({% post_url languages/erlang/2000-01-01-start %})

You need to have either a `rebar.config` file or a `ebin` file at the root of your project.