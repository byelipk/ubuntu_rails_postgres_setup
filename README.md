Set up a virtual environment for Rails development on Ubuntu 12.04LTS
=====================================================================

This is an aide-memoire on the steps to installing Rails v 4.0.2, Ruby v 2.0.0, and
Postgresql 9.3 on Ubuntu 12.04LTS running on a VMWare Player virtual machine. 

This is NOT meant to be comphrehensive, and is aimed at developers who have been working with ruby and rails
in a Windows environment, but now seek to switch over to a unix development environment.

Why use a virtual environment for rails development?
----------------------------------------------------

You may want to switch to a unix environment because there are many gems available that enhance your
workflow as a rails developer, and improve the performance of your application. However, getting them to
install on a Windows machine is either painful or impossible. 

Another reason is that more often than not, your application will be run in production on a unix machine, 
and it is good practice to develop in the environment your application will be run in. 

If you are starting out learning rails and or ruby, you will find that many of the quality screencasts and 
online tutorials available are written on a unix system (either Linux or Mac). Following along can be somewhat
tricky because are seeing all these different symbols and commands and directory structures that are not
applicable to your situation. Even the code snippets you find on Github, Heroku, and other rails books use unix symbology. 

Your brain has to filter these distractions out, abstract the useful information, and apply it to your situation 
in the Windows environment. The cost of that tiny extra amount of energy it takes for your brain to process all 
these additonal tasks takes it toll over time. 

Computer programming and web development are complicated enough subjects on their own. So, it is wise to do what 
you can to minimize the distractions you have control over. You will experience a much improved workflow, and will have 
much more fun, by following along these tutorials in the unix environment they were written in.


What to expect?
---------------

* Code snippets you can copy and paste into your unix terminal

* Particular hick-ups and gotchas you should expect to experience during the installation.

* An approximate time to complete some step in the process.


My host machine is a Lenovo T400 running Windows 7 32-bit. 

__Let's go!__




PART I - Set up the Ubuntu Virtual Machine
======
The first thing you need to do is download VMWare Player and Ubuntu from your host machine.

You can get the downloads here: 

* VMWare Player:    https://www.vmware.com/support/download-player.html

* Ubuntu 12.04 LTS: http://www.ubuntu.com/download/desktop
	
approx_time (15min) 


###VMWare Player Installation:
	
This is a fairly typical set up, so I will not go into detail. However, to save space I installed the VM on an external hard drive. 
My file structure looks like this: 		

`F:/VM`
	
When you load up VM Player, you can install Ubuntu!

approx_time (2min)


###Ubuntu Virtual Machine set up:		

Run the vmplayer.exe and go through the process of creating a new VM.
I chose to allocate 20gb of ram and 1gb of memory to the VM.

Ubuntu will install the first time you click `Run`. 

This takes some time, but you will have a nice, clean, fresh Ubuntu install. Yay!

approx_time(30-45min)


PART II - Set up your Ubuntu environment

	NOTE: As I am starting out on Ubuntu myself, many of the unix commands you see were taken from 
	this tutorial: http://gorails.com/setup/ubuntu/13.10
	
	It was very helpful to me and I suggest you check it out. As of this writing, they seem to be in the
	process of developing some screencasts. So, if you are new to rails/programming, it would be worth it to check
	them out.		

	The first thing you need to do once you have logged into Ubuntu is to enter the terminal
	and run: 
		
		<sudo apt-get update> 

	NOTE: You can find the terminal by moving your cursor over to the left-hand side of the screen and moving it
	up to the top-most icon named 'Dash.' Once you click on 'Dash' you will be brought to a search box where you can
	just enter 'terminal' and the icon for Ubuntu's command line interface will appear. Click on it, and you are inside
	the terminal.

	<apt> is Ubuntu's package manager, and you will be using it a lot.
	The command <sudo> gives you root user privilages. The entire command would fail without <sudo>.

	Once your system is updated, you can install the dependencies you will need.
	Copy the command inside the <> and paste into your terminal:

		<sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev 
		libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev>

	We will use Ruby Version Manager (RVM) to handle installing Ruby, as well as other gems. So, here we go!
	Paste these commands line-by-line into the terminal:
		
		<
		sudo apt-get install libgdbm-dev libncurses5-dev automake libtool bison libffi-dev
		curl -L https://get.rvm.io | bash -s stable
		source ~/.rvm/scripts/rvm
		echo "source ~/.rvm/scripts/rvm" >> ~/.bashrc
		rvm install 2.0.0-p353
		rvm use 2.0.0-p353 --default
		ruby -v
		>

	Essentially, we're downloading RVM (notice that it is not with <apt-get>), and than installing Ruby.		
	I will forgoe a breakdown of these commands for the moment, but will post it when I know exactly what is going on.
	
	Run this command to tell RubyGems not to install documentation for each package locally:

		<echo "gem: --no-ri --no-rdoc" > ~/.gemrc> 
				
	approx_time (5mins)


PART III - Configuring GIT version control

	You will need to create an account if you do not already have one. Go here: https://github.com/

	Before you run the following commands, just review this GitHub page on generating ssh keys: https://help.github.com/articles/generating-ssh-keys 
	
	Now you can run these commands:
		
		<
		git config --global color.ui true
		git config --global user.name "your_username"
		git config --global user.email "you@email.com"
		ssh-keygen -t rsa -C "you@email.com"
		>
	
	Make sure you are logged into GitHub, then navigate to this page:https://github.com/settings/ssh

	Run this command <cat ~/.ssh/id_rsa.pub> and use the output to add a new ssh key.

	Check to see if all of that actually worked by running the following command: 

		<ssh -T git@github.com>

	You should receive a response like this:

		Hi your_username! You've successfully authenticated, but GitHub does not provide shell access.

	You are good to go!

	approx_time (5mins)


PART IV - Installing Rails

	Installing Rails is quite simple compared to what we had to do in order to lay the ground work. But there is one thing
	we have to take care of before we can install rails.

	The Rails asset pipeline requires a javascript runtime in order for it to, for example, precompile Sass into CSS
	and CoffeeScript into JavaScript. The Rails 4 Gemfile comes with a pre-commented out command to install 'therubyracer' 
	gem, which would supply your Rails project with the necessary javascript runtime. But we can also install NodeJS, an interesting
	technology in its own right, giving us a global runtime. Because it is so simple just to uncomment <gem 'therubyracer'>, 
	we will do here go through installing NodeJS.

	Visit NodeJS to learn more: http://nodejs.org/

	Run these commands in order:

		<
		sudo add-apt-repository ppa:chris-lea/node.js
		sudo apt-get update
		sudo apt-get install nodejs
		>

	With your javascript runtime in place, now you can <gem install rails>. Yay! Now, quick... run <rails -v> to make sure it worked.

 
PART V - Installing Postgresql-9.3


	Run these commands:

		<
		sudo sh -c "echo 'deb http://apt.postgresql.org/pub/repos/apt/ precise-pgdg main' > /etc/apt/sources.list.d/pgdg.list"
		wget --quiet -O - http://apt.postgresql.org/pub/repos/apt/ACCC4CF8.asc | sudo apt-key add -
		sudo apt-get update		
		sudo apt-get postgresql-9.3 pgadmin3
		>	

	Normally, the first thing you do after installing Postgres is to try and set up a test rails application using <rails new myapp -d postgresql>
	When you run this command, bundler is going to automatically install the postgres gem <pg>. 
	
	This was a failing step for me until I ran this command to install a missing dependency:

		<sudo apt-get install libpq-dev>

	
	The next step is to create a user with a password.
		
		<sudo -u postgres psql newuser> will create a new user named "newuser" and send you into the postgresql terminal.

		<\password newuser> will allow you to create a password for your new user, which you will be asked to type into the terminal.
		

	There is just one last thing to do before you can create a test app and launch it, and it requires a text editor and a little knowledge
	of Ubuntu's file structure. 

	Ubuntu comes with <gedit> and <vim>. I am using vim right now, but it is a little complicated to navigate. I have been learning vim for
	3 or 4 hours, and am comfortable relying on it as my primary text editor. The version of <vim> that comes prepackaged with Ubuntu is 
	a lite version, so to upgrade to the full-featured version, run <sudo apt-get install vim> in the command line. 

	You should also pick up <gvim>, which sports a graphical user interface by running <sudo apt-get install gvim>. 

	If you are not familiar with vim, just use <gedit> for this step.

	Ok, now we want to make a test application to make sure we installed everything correctly. 

	Make sure you are in the home directory by running <cd ~/>. (Just to confirm, if you run <pwd>, it should say </home/whatever_your_username_is>.)


	Let's go into the Documents directory and create a Dev directory, into which we will put our rails test app. 

	Run these commands:

		<cd Documents>
		<mkdir Dev>
		<cd Dev>

	Now, let's make our rails test app by running: 

		<rails new myapp -d postgresql>

	
	Then run <cd myapp> in order to change into the myapp directory. 

	Run <cd config> to enter the config directory. Now type <gedit database.yml> or <vim database.yml> in order to open up the database 
	configuration file in your text editor of choice. 
	
	You should look for code like this for development, testing, and production environments:

		<
		development:
		  adapter: postgresql
		  encoding: unicode
		  database: myapp_development 
		  pool: 5
		  username: 
		  password: 	
		>

	There is a little bit of configuration we need to do so we do not run into any errors when we launch our app.

	Ensure that, for each environment, you include the username and password you used created previously in postgresql, 
	and that you set <host: localhost>. Your code should look like this:

		<
		development:
		  adapter: postgresql
		  encoding: unicode
		  database: myapp_development 
		  host: localhost
		  pool: 5
		  username: new_user
		  password: password	
		>


	Now, go back one level into your application's root directory by running <cd ../> and set up your database
	by running <rake db:create>. 

	If you see no indication of success or failure upon the completion of the command, that means it succeeded! Yay! 

	Now run <rails s> to start your server. When the server is loaded, fire up firefox and goto localhost:3000
	and you should see a page telling you that you are riding ruby on rails, or something like that... 

	Congrats!	

