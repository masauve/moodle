# Moodle on OpenShift

This branch is OpenShift ready!  Here's how to get Moodle up and running in your OpenShift environment.

What does this git repo have that the official repo does not?
1. An `/ocp` folder with build and deploy templates to easily get things running on OpenShift.
2. A `config.php` file in the root of the project, where each config value is mapped to an *environment variable*
3. A `/php-post-assemble` directory with a single `.sh` script to install themes and plugins.

## Create a New Project

Create a new project (e.g. `moodle-test`) where you will build and run Moodle.

## Start a PostgreSQL Container

In your Moodle project, start up a PostgreSQL instance.  Make sure you take not of the **service name, database name, user, and password**.

## Create the Build

### Through the UI

In your project:

1. Select `Add to Project -> Import YAML / JSON`
2. Paste in the contents of `/ocp/build.yaml`
3. Click `Create`, then `Continue` with `Process Template` checked.

By default, this repo and branch is pre-popoulated.  You can easily change this to your own repo and branch if you have copied over the `config.php` file and `php-post-assemble` directory from this repository into your own repo.

If you didn't already, click `Create`!

Now, if you go to `Builds -> Builds`, you should see a Moodle build that is in progress.  This will clone the git repo, run composer to build the app, then run the post-assemble script to download and install any extra plugins you want.

### Command Line

Coming soon!

## Launch Moodle

### Through the UI

In your project:

1. Select `Add to Project -> Import YAML / JSON`
2. Paste in the contents of `/ocp/moodle-template.yaml`
3. Click `Create`, then `Continue` with `Process Template` checked.
4. Change `Moodle URL` to the url you want moodle to be accessible at.  This will be your route.
5. Fill in the database details (host=postgresql service, dabase name, user, password).

Click `Create`.

Once the build from the previous step has completed, the Moodle container will start.   Once it has started, click the route URL and the "install" will begin.  Since all of the config options are already taken care of in the `ConfigMap` (`Resources -> ConfigMaps -> moodle-config`) the installer won't have to ask any questions about what URL to use, where the data dir is, databse details, etc...

Finish the install process, and you're ready to go.

## Command Line

Coming soon!

## Conclusion

With this process, the config is done through environment variables and plugins/themes are installed when the container image is built.  This means that when the container restarts, you don't lose any data, config, or plugins (which is sort of important).