# bedrock-docker-deployer
[Deployer](https://deployer.org/) recipes for containerized [Roots Bedrock](https://roots.io/bedrock/) databases, also supports [Roots Sage](https://roots.io/sage/)..

Docker is a very powerful environment and helps you to deploy images to your server. If you develop on docker but need to deploy to a classic server, you need to dump the DB from the container and send it to the server.   

This recipe helps you to use [florianmoser/bedrock-deployer](https://github.com/FlorianMoser/bedrock-deployer) with a containerized setup.

Maybe you are even trying to deploy Bedrock to a shared hosting. Depending on your hosting environment, this *may* be possible. Check out [florianmoser/plesk-deployer](https://github.com/FlorianMoser/plesk-deployer).

**A word of caution:** Make sure you have a backup of your local as well as your remote files, before experimenting with deployment recipes. Files might easily get overwritten when you provide wrong paths! You are solely responsible by using the recipes provided here.

## Who needs this
PHP developers who would like to deploy their containerized Bedrock applications to a non-containerized server using Deployer.

## Installation
Use Composer:

````
$ composer require tobinski/bedrock-docker-deployer
````

# Recipes
This package offers one recipes to help you deploy your database to a server. You need additional recipe from florianmoser/bedrock-deployer

## Bedrock DB
Provides tasks to export the database from the server and import it to your development container and vice versa.

Requirements:

- docker **running** on your local machine 
- WP CLI running on your container as well as on your remote machine

Load into your deploy.php file with:

````php
require 'vendor/tobinski/bedrock-docker-deployer/recipe/bedrock_dopcker_db.php';
````

Requires these Deployer environment variables to be set:

- local_root: Absolute path to website root directory on local host machine
- wp_container: The name of the Wordpress container
Example:

````php
set( 'local_root', dirname( __FILE__ ) );
set( 'wp_container', "wordpress );
````

### Task pull:db
Exports database on server and imports it into your local container, while removing previous data. Creates a backup of the local database in the local_root directory, before importing the new data.

After the import, the WordPress URLs are converted from server URL to local URL, so your WordPress installation will continue to work right after the import.

Database credentials and URLs are read from remote and local .env file. So make sure, those files are up to date.

### Task push:db
Exports database from local container and imports it into your remote server, while removing previous data. Creates a backup of the remote database on the server in the current release directory, before importing the new data.

After the import, the WordPress URLs are converted from local URL to remote URL, so your WordPress installation will continue to work right after the import.

Database credentials and URLs are read from remote and local .env file. So make sure, those files are up to date.
