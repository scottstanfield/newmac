Title: Vertigo developer setup on Mac
Author: Scott Stanfield
Date:   5/16/2012

---


Trick out your Mac, old-school Unix style
-----------------------------------------
The steps below assume you have a clean, fully patched Mac with Lion (10.7.4)
We used the little chip thing that you get from Apple to rebuild an older Mac.

Make sure your account is setup with admin privilages.  
After install, fully apply patches then continue.

Rename Computer
===============
By default, your computer probably has a name like John Smith's
Computer. Rename it easily from Terminal:

	$ scutil --set HostName new_hostname

I use my initials then some indicator of the machine type, like ss-mbp17
for a 17" MacBook Pro. 


Desktop
=======

* Check for updates
* Turn off Scroll direction:natural
* Dock on the left, remove unused icons
* Tweak keyboard repeat rate (key repeat=fast, delay=short)
* Turn on Firewall (Security & Preferences)

Chrome
======

	$ curl -O https://dl.google.com/chrome/mac/stable/GGRM/googlechrome.dmg ~/tmp/chrome.dmg

* Make it the default browser
* Add it to the dock, under Finder
* Install vimium extension (just Google for "vimium")

Other Extensions
	1Password
	AdBlock
	dotjs (remove)
	Google +1 Button
	JoinTabs (optional)
	SessionBuddy
	Syntaxtic

XCODE
=====
Note: for Mountain Lion, you may need to follow some extra steps. The
version of XCode is now 4.4. Read this first: https://gist.github.com/1860902

Most of the homebrew packages need the compiler (and header and libraries)
previously only available after a full 4GB Xcode install from AppStore, then
choosing the optional Command Line Tools from the preference pane. 

But Apple now allows the installation of the Command Line Tools
*without* needing to install Xcode. However you need a free Apple ID to
download the CLT. After that, head to [Apple Developer Tools][1] to
download. 

[1]: http://https://developer.apple.com/downloads/index.action

As of this writing, Command Line Tools for Xcode - Late March 2012 is current.

Note: If you have Xcode already and want to remove, uninstall with:
	$ sudo /Developer/Library/uninstall-devtools --mode=all

Full story at http://kennethreitz.com/xcode-gcc-and-homebrew.html

BREW
====
Install brew before changing shells since it requires /bin/sh.

Instructions located at http://mxcl.github.com/homebrew/. MacPorts users
should read [why they should switch to brew][3].

[3]: http://lostincode.net/blog/homebrew

Also, to make it easier to type, http://goo.gl/M3FcV maps to 
https://raw.github.com/mxcl/homebrew/master/Library/Contributions/install_homebrew.rb

Do all this from Terminal.app; we'll swap out to iTerm2 later.

	$ /usr/bin/ruby -e "$(/usr/bin/curl -fsSL http://goo.gl/M3FcV)"
	$ brew doctor
	$ brew install cowsay
	$ rehash
	$ cowsay "brew install worked!"

To satisfy the xcode-select "error" reported by brew doctor, see
https://github.com/mxcl/homebrew/issues/10245 and run

	$ sudo xcode-select -switch /usr/bin


Change default shell
====================
Change your shell from bash to tcsh (temporary).
	
    $ chsh -s /bin/tcsh

Log out, then back in. We'll change 

GIT and HUB (and Hg for vim compile)
====================================
Although Xcode tools come with git, we prefer to "get" the latest. Git
it?

	$ brew install git
	$ brew install wget
	$ brew install ack

These two are optional. Hub is a wrapper for git when talking to github. 
Mercurial might be needed for brew formulas that use Mercurial.

	$ brew install hub
	$ brew install mercurial

Solarized
=========

	$ mkdir -p ~/projects/repos
	$ cd ~/projects/repos
	$ hub clone altercation/solarized

Then follow instructions under iterm2-colors-solarized folder to add the color
profiles to iTerm 2. I create two named profiles called "solarized-dark" and 
"solarized-light", each set to the corresponding color profile and Monoco 18pt.
Light is my default.

iTerm 2
=======

* Install [iTerm2](www.iTerm2.com) - beta version is fine.
* Add two new profiles: "solarized-light" and "solarized-dark".  
  Set the color contrast profile to *low*, otherwise colors will be
  washed out.
* Delete the default profile.
* Set both color schemes to the appropriate Solarized files from previous step.
* I use 18 pt. Menlo for font. Window: columns=100, rows=35


SSH for GitHub
==============
You only need this step if you'll be pushing repos back to github, or if
you plan on using SSH anywhere. Instructions at [github][2].

[2]: (http://help.github.com/mac-set-up-git/

LESS
====
Brew does not install duplicates by default, so work with an alt fork

	$ brew tap homebrew/dupes
	$ brew install less 


RUBY
====
Lion comes with ruby 1.8.7. This will get at least version 1.9.3

	% brew install ruby

PYTHON
======
Sooner or later, you will need python, another fine language.
Install as "framework" per http://docs.python-guide.org/en/latest/starting/install/osx/

	% brew install python --framework

todo: install pip
todo: maybe not install virtualenv 
	http://kev.inburke.com/kevin/virtualenv-is-an-anti-pattern-for-beginners/

VIM
===
Install new version of vim (with +clipboard)

	$ md ~/tmp
	$ curl https://raw.github.com/gist/2581041 > ~/tmp/vim.rb
	$ brew install ~/tmp/vim.rb
	$ ln -s /usr/local/bin/vim /usr/local/bin/vi  # I still call it vi

join.me
=======


# NODE
	$ brew install node
	$ curl http://npmjs.org/install.sh | sh		# install npm

# Setup Perforce Visual Merge as gits visual tool (Mac OS only)
http://www.andymcintosh.com/?p=33
http://gitguru.com/2009/02/22/integrating-git-with-a-visual-merge-tool/
Install app from DMG. Add two 2 line sh scripts. Modify .gitconfig

CloudApp (optional)
===================

Makes sharing screenshots and links trivial.
(Enable autoupload of screenshots)
SHIFT-COMMAND-4



# PARTY ON NODE/EXPRESS/JADE
# http://dandean.com/nodejs-npm-express-osx/
# Add app.use(express.logger("dev"));  

# THEN SEND IT OFF TO HEROKU

Install hub (teaches git about github)
http://defunkt.io/hub/

Consider using Makefile for .dotfiles like
https://github.com/domnikl/.home


# TODO

move to sorin zsh
build .dotfiles with make
xcode update (reboot first)
look into janus

screen shot app

note: moved macports crap with this:
sudo mv /opt/local tmp/macports

# Comment .vimrc with this
https://github.com/jeffbuttars/Viming-With-Buttars#vimoptions

[iTerm2]:(www.iterm2.com)


# VIM Tutorials

## Tutorials

* Type `vimtutor` into a shell to go through a brief interactive
  tutorial inside VIM.
* Read the slides at [VIM: Walking Without Crutches](http://walking-without-crutches.heroku.com/#1).
* Watch the screencasts at [vimcasts.org](http://vimcasts.org/)
* Watch Derek Wyatt's energetic tutorial videos at [his site](http://www.derekwyatt.org/vim/vim-tutorial-videos/)
* Read wycats' perspective on learning Vim at
    [Everyone who tried to convince me to use vim was wrong](http://yehudakatz.com/2010/07/29/everyone-who-tried-to-convince-me-to-use-vim-was-wrong/)
* Read this and other answers to a question about vim at StackOverflow:
	  [Your problem with Vim is that you don't grok vi](http://stackoverflow.com/questions/1218390/what-is-your-most-productive-shortcut-with-vim/1220118#1220118)

JavaScript Good Parts
=====================
Lambda
Closure
Dynamic Objects
Loose Typing
Object Liberals
JSON

ZSH
===
NOTE: DON'T INSTALL ZSH YET.

zsh (pronounced zee-shell) might be a good replacement for tcsh (tenix c shell).
It has better TAB completion than tcsh, but the syntax is kinda weird.

TODO: resarch ZSH more.

	$ ps -p $$		# see your current shell
	$ brew install zsh
	$ sudo chsh -s /usr/local/bin/zsh scott

# Setup oh-my-zsh

	$ hub clone sorin-ionescu/oh-my-zsh ~/.oh-my-zsh
	$ cd ~/.oh-my-zsh
	$ git submodule update --init --recursive
	$ cp cp templates/zshrc.zsh ~/.zshrc
	$ sudo mv /etc/zshenv /etc/profile
	$ logout
