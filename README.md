# Mono Heroku Buildpack

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpack) for Mono that will run [Service Stack](https://github.com/ServiceStack/ServiceStack/) Self hosted APIs on port 8080. A compatible Service Stack example API can be found here: https://github.com/nover/service-stack-selfhost-heroku

It uses [nginx](http://www.mono-project.com/FastCGI_Nginx) as the web server and runs on Mono 3.2.3. 

This fork is heavily based on the works of https://github.com/friism/heroku-buildpack-mono and all credit should go to the previous creators of this build pack.

## Usage

This build pack will _only_ run Service Stack APIs that are running in the [self hosted container](https://github.com/ServiceStack/ServiceStack/wiki/Run-ServiceStack-as-a-daemon-on-Linux) listening on port 8080. Nginx is started on the port specified in the Heroku environment and is using the self hosted container as a reverse proxy.

Additional details and guides:

 * [Running ASP.NET and .NET console apps on Heroku](http://friism.com/running-net-on-heroku)
 * [Running Katana/OWIN apps on Heroku](http://friism.com/running-owin-katana-apps-on-heroku)
 * [Buildpack update to Mono 3.2 and more](http://friism.com/heroku-net-buildpack-update-to-mono-3-2-and-more)
 * [Heroku .NET buildpack now with nginx](http://friism.com/heroku-net-buildpack-now-with-nginx)

Example usage:

    $ heroku create --buildpack http://github.com/nover/heroku-buildpack-mono.git
    $ git push heroku master

The buildpack will build your solution and launch all .exe files found, so make sure that you only have one exe in your solution.

## TODO

* ~~Store buildoutput in $CACHE_DIR and do incremental builds (also won't cause NuGet packages to be re-downloaded)~~
* Less simplistic nginx setup, see [nginx buildpack](https://github.com/ryandotsmith/nginx-buildpack)
* Factor repeated code into functions like [PHP buildpack](https://github.com/CHH/heroku-buildpack-php/blob/master/bin/compile)
* Consider inserting environment variables and connectionstrings into Web.config
* Remove original source code before slug is tarred up
* Slim down Mono runtime to reduce slug size and build time
* Avoid copying Mono runtime to build /app and ${BUILD_DIR} during build
* Web.Release.config
* ~~Get bundling and minification to work (likely to be Win/Linux path issues)~~
* Figure out whether there's hope for EntityFramework (and reliance on `System.Data.Entity` and other)
* ~~Get default Visual Studio templates working (you just have to fix casing problems~~
* More Mono/XSP versions and ability to select version, like [Python buildpack](https://devcenter.heroku.com/articles/python-runtimes)
* Get Katana hosting working with System.Web Host
* Visual Basic!

## Pre-compiling Binaries

Use Anvil and [buildpack-inline](https://github.com/kr/heroku-buildpack-inline). The script used when building Mono is available here: https://github.com/friism/build-mono
