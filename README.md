#  Development Mac Setup

Follow these instructions for setting up a new Mac for web development purposes.

_Updated 04/09/2019_

## 1. Update macOS

Before proceeding with anything, make sure macOS is up to date by going to **System Preferences** &rarr; **Software Update**.

## 2. Install Command Line Tools (without Xcode)

You can install the essential CLI tools without installing the full Xcode app. To do so, open Terminal and run:

	xcode-select --install
	
You will be prompted to confirm the installation. Click **Install** and agree to the terms.
	
If you need the full Xcode app, install that instead from the App Store.

## 3. Configure Command Line

### YADR (optional, recommended)

[YADR](https://github.com/skwp/dotfiles) is a set of vim, git, and zsh plugins that make using the CLI (Terminal or iTerm) more enjoyable. To install the full set of plugins, run the following:

	sh -c "`curl -fsSL https://raw.githubusercontent.com/skwp/dotfiles/master/install.sh`"
	
### ~/.zshrc Defaults

Add some useful defaults to your `~/.zshrc`:

- `ulimit -n 10240` - Bumps the maximum number of file descriptors you can have open on your computer. There's no purpose for the default limit, especially on SSDs
- `export JOBS=max` - Tells npm to compile and install all your native addons in parallel and not sequentially, which greatly increases installation timess see [https://github.com/skwp/dotfiles](https://github.com/skwp/dotfiles)

### agnoster zsh theme (optional)

For an even nicer cli experience, you can install [agnoster.zsh-theme](https://github.com/agnoster/agnoster-zsh-theme).


## 4. Check Homebrew
Once YADR has installed, run `brew doctor` to see if there are any issues with Homebrew. If there are, follow the provided instructions to correct them.

ℹ️ **Note:** Can't `chown /usr/local`? [Click here](https://github.com/Homebrew/brew/issues/3228#issuecomment-332679274)

If you have already migrated your SSH key you can feel free to continue, if not let's create one.


## 5. SSH keys
In order to connect to any of our LiquidWeb, Eleven2, or DigitalOcean servers over SSH/SFTP or commit to our Think Patented Github you must first have an SSH Key installed in your **~/.ssh/** directory.

To generate a key, paste the following in a terminal session:

	cd ~/.ssh && ssh-keygen -t rsa -b 4096 -C "youremailaddress@thinkpatented.com"

You will be asked to name the key, don't and just hit `enter` to leave as default **id_rsa** this will insure that any provided config files are pointing to the appropriate key. You will also be asked to provide a passphrase for the key, this is optional but recommended for an additional layer of security.

When the key has been created there will be two files (**id_rsa** and **id_rsa.pub**) The .pub file is your public key and can be shared freely. The non .pub file is your **private** key and should <span style="color:red">**NEVER**</span> be shared. Send your **id_rsa.pub** file to the administrators for inclusion as an authorized key on any appropriate servers/services.

ℹ️ **Note:** If you are encountering any issues connecting to a server, re-run the SSH command and include the "-v" flag to get verbose output which will provide more details on what is causing the issue. If you are unable to diagnose the issue yourself send the verbose output to the administrator for further diagnosis and to ensure your SSH key has been added as an authorized key on the server/service.

### SSH Config

An SSH config file (~/.ssh/config) is a huge timesaver so make sure you have one:

Bare minimum:

	Host *
     ForwardAgent no
     ForwardX11 no
     ForwardX11Trusted yes
     Protocol 2
     ServerAliveInterval 60
     ServerAliveCountMax 30
     Port 22
     PubKeyAuthentication yes
     IdentityFile ~/.ssh/id_rsa
    
    ##Example Hosts
    Host examplewebsite.com examplewebsite example
    	HostName 111.111.11.11
    	User example
    
    Host examplewebsite2.com examplewebsite2 example2
    	HostName 111.111.11.11
    	User example2

Benefits are:

 * instead of typing `ssh example@111.111.11.11` everytime you want to SSH, you can simply type `ssh [hostname]`, for example:
  * `ssh examplewebsite.com`
  * `ssh examplewebsite`
  * `ssh example`
 * Apps like **Transmit** will automatically look at this file and use its configs

## 6. Git
You'll need to set your git username globally so that all repo configs default to your username. Run the following:

	git config --global user.name "YOUR.USERNAME"
	git config --global user.email "youremail@address.com"

Also, under your Github's account settings you should add in your SSH key (id_rsa.pub) after which you can test your connection / add Github to your "known\_hosts" by running:

	ssh -T git@github.com

If this is the first time you've tried connecting to Github over SSH you'll be prompted to add it to your known\_hosts, select **yes**.
	
## 7. Install Homebrew packages

Homebrew should have been installed if you installed YADR above. Run the following scripts to install the first batch of essential tools and utilities:

	brew install rbenv pyenv nodenv the_silver_searcher ack z zsh awscli wget wpcli-completion dnsmasq git-town tree sqlite mariadb mcrypt nginx shellcheck pngquant mozjpeg tmux reattach-to-user-namespace composer wp-cli ffmpeg
	
Finally, install [Yarn](https://yarnpkg.com/en/) without Node so we can use the version of node that we'll install with nodenv
	
	brew install yarn --ignore-dependencies

## 8. Install Environment Tools

### RBENV

First, go to [ruby-lang.org](https://www.ruby-lang.org/en/downloads/) to find out the latest stable version of Ruby (2.6.2 at the time of writing).

Run `brew install rbenv` if you haven't already, then make sure the following has been added to your **~./zshrc**:

	export PATH="$HOME/.rbenv/bin:$PATH"
	eval "$(rbenv init -)"

Run the following to install and configure the Ruby version:

	rbenv install 2.6.2
	rbenv global 2.6.2
	
For more information on using rbenv, run `rbenv --help` or go to [github.com/rbenv/rbenv](https://github.com/rbenv/rbenv) for further documentation.

### NODENV

First, go to [nodejs.org](https://nodejs.org/en/) to find out the latest stable LTS (Long Term Support) version of Node (10.15.3 at the time of writing).

Run `brew install nodenv` if you haven't already, then make sure the following has been added to your **~./zshrc** :

	export PATH="$HOME/.nodenv/bin:$PATH"
	eval "$(nodenv init -)"
	
Run the following to install and configure the Node version:

	nodenv install 10.15.3
	nodenv global 10.15.3
	
For more information on using nodenv, run `nodenv --help` or go to [github.com/nodenv/nodenv](https://github.com/nodenv/nodenv) for further documentation.

### PYENV (optional)

First, go to [python.org](https://www.python.org/downloads/) to find out the latest stable version of Python (3.7.3 at the time of writing).

Run `brew install pyenv` if you haven't already, then make sure the following has been added to your **~./zshrc**:

	export PATH="$HOME/.pyenv/bin:$PATH"
	eval "$(pyenv init -)"
	
Run the following to install and configure the Python version:

	pyenv install 3.7.3
	pyenv global 3.7.3
	
For more information on using nodenv, *un `pyenv --help` or go to [github.com/pyenv/pyenv](https://github.com/pyenv/pyenv) for further documentation.

**NOTE:** On High Sierra you may run into an issue with OpenSSL not being found. Adding the following alias somewhere in your $PATH (ie: **~/.zshrc**) will tell pyenv where to find the homebrew installed OpenSSL lib.

	alias pyenv='CFLAGS="-I$(brew --prefix openssl)/include" LDFLAGS="-L$(brew --prefix openssl)/lib" pyenv'


### Composer

Make sure the following has been added to your **~./zshrc**:

	export PATH="$PATH:$HOME/.composer/vendor/bin"
	
Composer should have been installed with the other Homebrew packages above.
	
### Profile (~/.zshrc)
By this point in time you should have a **~/.zshrc** file that contains the following:

	export PATH="$HOME/.rbenv/bin:$PATH"
	export PATH="$HOME/.nodenv/bin:$PATH"
	export PATH="$HOME/.pyenv/bin:$PATH"
	export PATH="$PATH:$HOME/.composer/vendor/bin"
	
	eval "$(rbenv init -)"
	eval "$(nodenv init -)"
	eval "$(pyenv init -)"
	
	## if on MacOS 10.13 High Sierra
	alias pyenv='CFLAGS="-I$(brew --prefix openssl)/include" LDFLAGS="-L$(brew --prefix openssl)/lib" pyenv'
	
This will insure that your installed versions of ruby (via rbenv), node (via nodenv), python (via pyenv), and composer are all initialized and available every time you open a terminal session and avoid the dreaded "Command not Found"

## 9. Installing Cask Apps

Homebrew-Cask is an extension of Homebrew and allows you to manage macOS apps from a single source. You can search the directory for an app by running `brew cask search [app name]`.

Below is a list of required and helpful apps. To install all apps in the list, run the following command (you can also add or remove any):

	brew cask install imageoptim iterm2 qlcolorcode qlimagesize qlmarkdown qlprettypatch qlstephen qlvideo quicklook-csv quicklook-json sequel-pro
	
	
[Download Hosts.prefpane here](https://github.com/specialunderwear/Hosts.prefpane/downloads) as it's no longer available as a Cask.

## 10. The Web Stack

The following are the tools to get up and coding WordPress sites locally.

### Valet

First, assuming that composer has been installed correctly, run:

	composer global require laravel/valet
	
Once installed, run:

	valet install
	
Valet is now installed. Before your local environment is functional, you need to start all of the Homebrew services (and make them start at login):

	brew services start --all
	
This will start dnsmasq, mariadb, nginx, and php. You should now be able to run `ping foobar.test` and get a return of 127.0.0.1 which indicates the services are installed and running properly.

With Valet installed you can now `cd` into a project's intended webroot and run `valet link domainname` for example:

	cd ~/Sites/foobar/web
	valet link foobar
	
	A [foobar] symbolic link has been created in [~/.valet/Sites/foobar].
	
At which point you should be able to visit http://foobar.test in your browser and see *something*.

### MySQL

If you are developing a site that utilizes a MySQL database and you've already **A)** installed **mariadb** or **mysql** via homebrew and **B)** installed the **Sequal Pro** app...

1. Open up Sequal Pro
2. Enter your primary database information, which is likely:
	* **Host:** 127.0.0.1
	* **Username:** root
	* **Password:** (empty by default)
3. Create a new database for your local dev site
	* For team consistency, name the database `wp_[sitename]`, such as `wp_foobar`

You can now connect your site to the local databse using the info above.

**NOTE** that in your `.env` file for a WordPress site, uncomment out the `DB_PREFIX` line and specify a 6-character prefix, such as `wp_k28ah1_`. Do that **before** setting up WordPress so its created using those table prefixes. It's not recommended to change the table prefix after WordPress setup. This is key due to the fact that your dev, staging, and production sites need to have the same table prefix in order to migrate via MigrateDB Pro.

### Bedrock

More to come on this... We use Bedrock as the structure for our WordPress installs.

For more info see [https://github.com/roots/bedrock](https://github.com/roots/bedrock)

### Capistrano
More to come on this.. We use Capistrano and Ruby scripts to deploy our WordPress sites to staging and production servers.

For more info see: [https://github.com/capistrano/capistrano](https://github.com/capistrano/capistrano) and [https://github.com/roots/bedrock-capistrano](https://github.com/roots/bedrock-capistrano) (note: Bedrock's default Capistrano configs must be tweaked to work with most hosts)

