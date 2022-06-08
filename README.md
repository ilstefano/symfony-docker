# Symfony-Docker: A dockerized Symfony Fast Track Development Environment

This is a base PHP-Symfony development environment for e.g. the [Symfony Fast Track Book](https://symfony.com/book).

## Preliminaries

Your host must have ```docker``` and ```docker compose``` installed. Whether your host is Linux or WSL or Mac doesn't matter.

## Usage

Checkout the repo and cd into the working dir.

Next the provided docker-compose platform here will run best in Visual Studio Code, see [Visual Studio Code](#visual-studio-code).

You may also easily run it on the command line, see [CLI](#cli)

If you use another IDE (like PHPStorm) please try to find out yourself how the docker-compose fits into your setup.

### Provide some build args

We need to inject build args via the ```.env``` file first, such that the container user will be the same as in the host and has access to the hosts's docker engine:

```bash
cd .docker
echo "USERID=$(id -u)" > .env
echo "DOCKERGID=$(getent group docker | cut -d: -f3)" >> .env
echo "USER=${USER}" >> .env
```

### CLI

```bash
# in .docker

docker compose build
docker compose run app bash
```

Or you run the app conatiner detached and enter it as you want:

```bash
# in .docker

docker compose up -d

docker compose exec app bash
```

### Visual Studio Code

VSC is extremely seamlessly supporting local development by ['remote containers'](https://code.visualstudio.com/docs/remote/containers) (as they are remote with respect to the host the IDE is running on).

Just open the project root folder in VSC and switch to 'Reopen in Devcontainer'.

## Outcome

### Symfony CLI

In the running container or in a VSC remote's session terminal you now may run ```symfony``` console commands like this:

```bash
stl@stl-symfony /workspace (main) $ symfony

 INFO  A new Symfony CLI version is available (5.4.7, currently running 5.4.5).

       If you installed the Symfony CLI via a package manager, updates are going to be automatic.
       If not, upgrade by downloading the new version at https://github.com/symfony-cli/symfony-cli/releases
       And replace the current binary (symfony) by the new one.

Symfony CLI version 5.4.5 (c) 2017-2022 Symfony SAS #StandWithUkraine Support Ukraine (2022-03-25T14:41:32Z - stable)
Symfony CLI helps developers manage projects, from local code to remote infrastructure

These are common commands used in various situations:

Work on a project locally

  new                                                            Create a new Symfony project
  server:start                                                   Run a local web server
  server:stop                                                    Stop the local web server
  security:check                                                 Check security issues in project dependencies
  composer                                                       Runs Composer without memory limit
  console                                                        Runs the Symfony Console (bin/console) for current project
  php, pecl, pear, php-fpm, php-cgi, php-config, phpdbg, phpize  Runs the named binary using the configured PHP version

Manage a project on Cloud

  init                Initialize a new project using templates
  cloud:domains       Get a list of all domains
  cloud:branch        Branch an environment
  cloud:environments  Get a list of environments
  cloud:push          Push code to an environment
  cloud:ssh           SSH to the current environment
  cloud:projects      Get a list of all active projects
  cloud:tunnel:open   Open SSH tunnels to an app's relationships
  cloud:user:add      Add a user to the project
  cloud:variables     List variables

Show all commands with symfony help,
Get help for a specific command with symfony help COMMAND.
stl@stl-symfony /workspace (main) $ 
```

### Install [Fast Track project](https://symfony.com/doc/6.0/the-fast-track/en/2-project.html)

```bash
stl@stl-symfony /workspace (main) $ symfony new --version=6.0-1 --book guestbook

 INFO  A new Symfony CLI version is available (5.4.7, currently running 5.4.5).

       If you installed the Symfony CLI via a package manager, updates are going to be automatic.
       If not, upgrade by downloading the new version at https://github.com/symfony-cli/symfony-cli/releases
       And replace the current binary (symfony) by the new one.

Checking Book Requirements
--------------------------

[OK] Git installed
[OK] PHP installed version 8.1.4 (/usr/local/bin/php)
[OK] PHP extension "json" installed - required
[OK] PHP extension "session" installed - required
[OK] PHP extension "intl" installed - required
[OK] PHP extension "openssl" installed - required
[KO] PHP extension "amqp" not found, optional - needed only for chapter 32
[OK] PHP extension "pdo_pgsql" installed - required
[OK] PHP extension "mbstring" installed - required
[OK] PHP extension "xsl" installed - required
[OK] PHP extension "curl" installed - optional - needed only for chapter 17 (Panther)
[KO] PHP extension "redis" not found, optional - needed only for chapter 31
[OK] PHP extension "tokenizer" installed - required
[OK] PHP extension "xml" installed - required
[OK] PHP extension "ctype" installed - required
[OK] PHP extension "sodium" installed - required
[OK] PHP extension "zip" installed - optional - needed only for chapter 17 (Panther)
[OK] PHP extension "gd" installed - optional - needed only for chapter 23 (Imagine)
[OK] Composer installed
[OK] Docker installed
[OK] Docker Compose installed
[OK] Yarn installed

Cloning the Repository
----------------------

Cloning into '/workspace/guestbook'...
remote: Enumerating objects: 1084, done.
remote: Counting objects: 100% (1084/1084), done.
remote: Compressing objects: 100% (556/556), done.
remote: Total 1084 (delta 493), reused 1084 (delta 493), pack-reused 0
Receiving objects: 100% (1084/1084), 4.35 MiB | 2.56 MiB/s, done.
Resolving deltas: 100% (493/493), done.

Getting Ready for the First Step of the Book
--------------------------------------------

[GIT] Check for not yet committed changes: [ OK ]
[GIT] Check Git un-tracked files: [ OK ]

[GIT] Removing Git ignored files (vendor, cache, ...): [ OK ]
[GIT] Resetting Git staged files: [ OK ]
[GIT] Removing un-tracked Git files: [ OK ]
[WEB] Adding .env.local: [ OK ]
[WEB] Stopping Docker Containers: [ OK ]
[WEB] Stopping the Local Web Server: [ OK ]
[WEB] Stopping the Platform.sh tunnel: [ OK ]
[GIT] Checking out the step: [ OK ]
[SPA] Stopping the Local Web Server: Skipped for this step
[WEB] Installing Composer dependencies (might take some time): [ OK ]
[WEB] Adding .env.local: [ OK ]
[WEB] Starting Docker Compose: [ OK ]
[WEB] Waiting for the Containers to be ready: [ OK ]
[WEB] Migrating the database: Skipped for this step
[WEB] Inserting Fixtures: Skipped for this step
[WEB] Installing Node dependencies (might take some time): [ OK ]
[WEB] Building CSS and JS assets: [ OK ]
[WEB] Starting the Local Web Server: [ OK ]
[WEB] Starting Message Consumer: Skipped for this step
[SPA] Installing Node dependencies (might take some time): Skipped for this step
[SPA] Building CSS and JS assets: Skipped for this step
[SPA] Starting the Local Web Server: Skipped for this step

              
 [OK] All done!

stl@stl-symfony /workspace (main) $ 
```

### Start webserver

```bash
stl@stl-symfony /workspace/guestbook (work-step-3) $ symfony server:start -d

 INFO  A new Symfony CLI version is available (5.4.7, currently running 5.4.5).

       If you installed the Symfony CLI via a package manager, updates are going to be automatic.
       If not, upgrade by downloading the new version at https://github.com/symfony-cli/symfony-cli/releases
       And replace the current binary (symfony) by the new one.

                                                                                                                        
 [WARNING] run "symfony server:ca:install" first if you want to run the web server with TLS support, or use "--p12" or  
  "--no-tls" to avoid this warning                                                                                      
                                                                                                                        

                                                                                                                        
 [WARNING] The local web server is optimized for local development and MUST never be used in a production setup.        
                                                                                                                        

                                                                                                                        
 [OK] Web server listening                                                                                              
      The Web server is using PHP FPM 8.1.4                                                                             
      http://127.0.0.1:8000                                                                                             
                                                                                                                        

Stream the logs via symfony server:log
stl@stl-symfony /workspace/guestbook (work-step-3) $ 
```
