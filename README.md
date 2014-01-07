Set up a virtual environment for Rails development on Ubuntu 12.04LTS
=====================================================================

This is an aide-memoire on the steps to installing Rails v 4.0.2, Ruby v 2.0.0, and
Postgresql 9.3 on Ubuntu 12.04LTS running on a VMWare Player virtual machine. 

This is aimed at developers who have been working with ruby and rails
in a Windows environment, but now seek to switch over to a unix development environment.

Why use a virtual environment for rails development?
----------------------------------------------------

###In general

You may want to switch to a unix environment because there are many gems available that enhance your
workflow as a rails developer, and improve the performance of your application. However, getting them to
install on a Windows machine is either painful or impossible. 

Another reason is that more often than not, your application will be run in production on a unix machine, 
and it is good practice to develop in the environment your application will be run in. 

###For the beginner

If you are starting out learning rails and or ruby, you will find that many of the quality screencasts and 
online tutorials available are written on a unix system (either Linux or Mac). Following along can be tedious
because some of the essential symbols, commands, and directory structures used are not applicable to your Windows 
environment.  

Computer programming and web development are complicated enough subjects on their own. So, it is wise to do what 
you can to minimize the distractions you have control over. You will experience a much improved workflow, and will have 
much more fun, by following along these tutorials in the unix environment they were written in. 


What to expect?
---------------


* Code snippets you can copy and paste into your unix terminal

* Particular hick-ups and gotchas you should expect to experience during the installation.

* An approximate time to complete some step in the process.


*My host machine is a Lenovo T400 running Windows 7 32-bit.* 


__Ok, let's go!__




PART I - Set up the Ubuntu Virtual Machine
==========================================

The first thing you need to do is download VMWare Player and Ubuntu from your host machine.

You can get the downloads here: 

* VMWare Player:    https://www.vmware.com/support/download-player.html

* Ubuntu 12.04 LTS: http://www.ubuntu.com/download/desktop
	
*approx_time (15min)* 


###VMWare Player Installation:
	
This is a fairly typical set up, so I will not go into detail. However, to save space I installed the VM on an external hard drive. 
As a result, my file structure looks like this: 		

`F:/VM`
	
When you load up VM Player, you can install Ubuntu!

*approx_time (2min)*


###Ubuntu Virtual Machine set up:		

* Run the vmplayer.exe and go through the process of creating a new virtual environment.

* You also need to reference the file location of the Ubuntu .iso you downloaded. 

* I chose to allocate 20gb of ram and 1gb of memory to the VM.

Ubuntu will install the first time you click `Run`. 

The complete installation takes some time, but you will have a nice, clean, fresh Ubuntu install. Yay!

*approx_time(30-45min)*


PART II - Set up your Ubuntu environment
========================================

NOTE: As I am starting out on Ubuntu myself, many of the unix commands you see were taken from 
this tutorial: http://gorails.com/setup/ubuntu/13.10

It was very helpful to me and I suggest you check it out.		

###Update your system

The first thing you need to do once you have logged into Ubuntu is to enter the terminal
and run: 

`sudo apt-get update` 

*NOTE: You can find the terminal by moving your cursor over to the left-hand side of the screen and moving it
up to the top-most icon named 'Dash.' Once you click on 'Dash' you will be brought to a search box where you can
just enter 'terminal' and the icon for Ubuntu's command line interface will appear. Click on it, and you are inside
the terminal.*

`apt` is Ubuntu's package manager, and you will be using it a lot.
The command `sudo` gives you root user privilages. The entire command would fail without `sudo`.

Once your system is updated, you can install the dependencies you will need.

###Install additional dependencies

Copy the command inside the below and paste it into your terminal:

	sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev 
	libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev

*Note: you can copy and paste this code snippet into one line in your terminal. I added another row
just for readability purposes.* 

###Install RVM

We will use Ruby Version Manager (RVM) to handle installing Ruby, as well as other gems.

Paste these commands line-by-line into the terminal:

		
	sudo apt-get install libgdbm-dev libncurses5-dev automake libtool bison libffi-dev
	curl -L https://get.rvm.io | bash -s stable
	source ~/.rvm/scripts/rvm
	echo "source ~/.rvm/scripts/rvm" >> ~/.bashrc
	rvm install 2.0.0-p353
	rvm use 2.0.0-p353 --default
	ruby -v


Essentially, we're downloading RVM (with cURL, not apt-get), and then installing Ruby.
		
Run this command to tell RubyGems not to install documentation for each package locally:

	`echo "gem: --no-ri --no-rdoc" > ~/.gemrc` 
			
*approx_time (5mins)*


PART III - Configuring GIT version control
==========================================

Note: You will need to create an account if you do not already have one. Go here: https://github.com/

Before you run the following commands, just review this GitHub page on generating ssh keys: https://help.github.com/articles/generating-ssh-keys 

Now you can run:

	git config --global color.ui true
	git config --global user.name "your_username"
	git config --global user.email "you@email.com"
	ssh-keygen -t rsa -C "you@email.com"

Make sure you are logged into GitHub, then navigate to this page:https://github.com/settings/ssh

Run `cat ~/.ssh/id_rsa.pub` and paste the output into the "Key" field, then give your key a descriptive title.

Check to see if all of that actually worked by running: `ssh -T git@github.com`

If you receive a response like this:

	Hi your_username! You've successfully authenticated, but GitHub does not provide shell access.

then you are good to go!

*approx_time (5mins)*


PART IV - Installing Rails
==========================

Installing Rails is quite simple compared to what we had to do in order to lay the ground work. But there is one thing
we have to take care of before we can install rails.

The Rails asset pipeline requires a javascript runtime in order for it to, for example, precompile Sass into CSS
and CoffeeScript into JavaScript. The Rails 4 Gemfile comes with a pre-commented out command to install 'therubyracer' 
gem, which would supply your Rails project with the necessary javascript runtime. But we can also install NodeJS, 
an interesting technology in its own right, which gives us a global javascript runtime. 

Because it is so simple just to uncomment `gem 'therubyracer'`, we will go through installing NodeJS.

Visit NodeJS to learn more: http://nodejs.org/

Run these commands in order:

	
	sudo add-apt-repository ppa:chris-lea/node.js
	sudo apt-get update
	sudo apt-get install nodejs
	
Now run `node -v` to confirm that Node.js was installed properly.

With your javascript runtime in place, now you can `gem install rails`.

Yay! Now, quick... run `rails -v` to make sure it worked.


PART V - Installing Postgresql-9.3
==================================

Run these commands to install PostgresQL 9.3 and PGAdmin 3, a graphical user interface for your databases:

	
	sudo sh -c "echo 'deb http://apt.postgresql.org/pub/repos/apt/ precise-pgdg main' > /etc/apt/sources.list.d/pgdg.list"
	wget --quiet -O - http://apt.postgresql.org/pub/repos/apt/ACCC4CF8.asc | sudo apt-key add -
	sudo apt-get update		
	sudo apt-get install postgresql-9.3 pgadmin3
	
*Note that on Ubuntu, PostgreSQL will be installed in `/usr/share/postgresql`*

Now we need to install a dependency that will allow Bundler to run `gem install 'pg'` automatically when we set up
a new rails app with PostgreSQL.

	sudo apt-get install libpq-dev


### Create new rails app

Let's try and create a sample application with a PostgreSQL database just as a
sanity check. Make sure you are in the home directory by running `cd ~/`. 
(Just to confirm, if you run `pwd`, it should say `/home/whatever_your_username_is`.)

Let's go into the Documents directory and create a Dev directory, into which we will put our rails test app. 

Run these commands:

	cd Documents
	mkdir Dev
	cd Dev

Run this command in the terminal: `rails new test_app -d postgresql`

Change into the `test_app` directory and run `rails start`. You should see the
server start up normally. 

Now open up your web browser and navigate to `localhost:3000`.

You should see an error similar to `PG::ConnectionBad` with a message like
`FATAL: Peer authentication failed for user 'test_app'`

This error happened because when we generated the new rails app, rails used
our application name, `test_app`, to create a new database. In other words, rails
assumed that `test_app` was a valid PostgeSQL user with certain privilages -
namely that `test_user` could create a new database. But we never created a new
user with the power to create a new database.

###Configure PostgreSQL

As far as resolving the `PG::ConnectionBad` is concerned, there are two ways we can handle
this. We can either modify a file in postgres' directory structure, or modify
`database.yml` in the `config` folder in your rails app. The latter is
considerably easier, so I will walk you through the more involved method and
make a note at the bottom of this guide that walks you through the *easy* way.

Just to recap what we are about to do, we are going to

* modify a file within PostgreSQL's directory structure called `pg_hba.conf` 

* create a new `test_app` role within PostgreSQL with the proper privilages 

* and then modify a file named `database.yml` in our application's `config` 
folder with our new login information 

First, you need to stop the rails server by pressing `CTRL-c`.

To modify `pg_hba.conf` we are going to need to work in the PostgreSQL command
line interface. In the terminal type `sudo -u postgres psql`. This logs you in
as the `postgres` superuser. Now that you are in the CLI you can run queries 
against the database. What we need to query right now is the location of 
`pg_hba.conf` so we can modify it. Run this query: 

	SHOW hba_file;

You should now see a path to the file location. 

Mine was `/etc/postgresql/9.3/main/pg_hba.conf`.

If your path is the same as mine, you would type `cd /etc/postgresql/9.3/main/`
to change into the directory. In order to edit the file, you will need
`SUPERUSER` privilages, which you get when you use `sudo`. 

Open the file by typing `sudo gedit pg_hba.conf`.

Scroll to the bottom of the file where you will make one change. You should see
a table structure like this:

	# Type	Database	USER	ADDRESS		METHOD
	  ----------------------------------------------------
	  local all		all			peer

Where you see `peer`, change this one instance of `peer` to `md5` then save the
file.

When you are back on the command line you need to restart PostgreSQL by running
`sudo service postgresql restart`.

### Create a new role

We are going to drop back into the PostgreSQL command line to create a new
user "role".

Run: `sudo -u postgres psql postgres`

Remember, we need to give this role certain privilages. First, it needs to be
able to create a new database, and it needs to be able to login with a password.
We are going to follow rails conventions and name our role `test_app'. To make 
all this happen we run this SQL query:

	CREATE ROLE test_app CREATEDB LOGIN PASSWORD 'secret';

With the new role created, you can log out of the CLI with `\q` and return to
the root directory of your rails application.

### Modify database.yml

Change into the application's `config` folder by running `cd config` and open up the database
configuration file by running `gedit database.yml`.

*GEDIT is a text editor that comes packaged into an Ubuntu installation.*

If you examine the file you will notice that rails is trying to configure
databases for three kinds of *environments* - development, test, and production.
Let's take a quick look at the configuration settings for the development
environment.

	development:
	  adapter: postgresql
	  encoding: unicode
	  database: test_app_development
	  pool: 5
	  username: test_app
	  password:

The two things to point out right off the bat are the settings for `database` and
`username`. Here we see that rails wants to create a database called
`test_app_development` and wants to connect to it with a username called
`test_app`.

Ensure that, for each environment, you include the username and password you used created previously in postgresql, 
and that you set `host: localhost`. Your code should look like this:
	
	development:
	  adapter: postgresql
	  encoding: unicode
	  database: test_app_development 
	  pool: 5
	  username: test_app 
	  password: secret	

Now, go back one level into your application's root directory by running `cd ../` and set up your database
by running `rake db:create`. 

If you see no indication of success or failure upon the completion of the command, that means it succeeded! Yay! 

Now run `rails s` to start your server. When the server is loaded, fire up firefox and goto localhost:3000
and you should see a page telling you that you are riding ruby on rails, or something like that... 

Congrats!	

APPENDIX-A
----------

Remember when I said there was an easy way to resolve the `PG::ConnectionBad`
error you saw upon navigating to `localhost:3000`? Well, here it is...

All we needed to do was make a tiny modification to `database.yml`.

	development:
	  adapter: postgresql
	  encoding: unicode
	  database: test_app_development 
	  host: localhost
	  pool: 5
	  username: test_app 
	  password: secret	

Notice that I added `host: localhost` in the configuration settings. This has
the same effect as does modifying `pg_hba.conf`!  

However, if you expect to be using the CLI at all, I reccomend against this
method because it does not give your user the ability to enter the command line.

For example, if you had not modified `pg_hba.conf`, you would not be able to log
into your database like so: `psql -d test_app_development -U test_app -W`

Again, if you have no need to modify your database via the command line, then
this streamlined method will be fine. Otherwise, it is worth the extra
configuration.

APPENDIX-B
----------

To enable hstore in your database, you need to first install the postgresql
extension.

	sudo apt-get install postgresql-contrib

Now the `hstore` extension should be available so you can install it like so:

	sudo -u postgres psql test_app_development
	CREATE EXTENSION hstore;

You must repeat this step for the test database as well. 

Now you can use the hstore data type.

Text Editors	
------------

There is just one last thing to do before you can create a test app and launch it, and it requires a text editor and a little knowledge
of Ubuntu's file structure. 

Ubuntu comes with `gedit` and `vim`. I am using `vim` right now. However, `vim` takes some practice. I have been learning `vim` for
3 or 4 hours, and am comfortable relying on it as my primary text editor. The version of `vim` that comes prepackaged with Ubuntu is 
a lite version, so to upgrade to the full-featured version, run `sudo apt-get install vim` in the command line. 

You should also pick up `gvim`, which sports a graphical user interface by running `sudo apt-get install gvim`. 

If you are not familiar with `vim`, just use `gedit` for this step.

