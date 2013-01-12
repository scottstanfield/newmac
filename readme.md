Title: Vertigo developer setup on Mac
Author: Scott Stanfield
Date:   January 2013

---

Mac Config for Web Development
==============================
Tested for *Mountain Lion* (OSX 10.8).

The steps below assume you have a clean, fully patched Mac with Mt Lion (10.8.x).
We used the little chip thing that you get from Apple to rebuild an older Mac.

Make sure your account is setup with admin priveleges.  
After install, fully apply patches then continue.

This gist will help you get X11 working with Mountain Lion since it's no
longer installed with OS X: https://gist.github.com/1860902

This guide was inspired by Mr Belyamani's 
[Ruby focused guide](http://bit.ly/VQsHy1).

Rename Computer
---------------
By default, your computer probably has a name like John Smith's
Computer. Rename it easily from Terminal:

    $ scutil --set HostName new_hostname
    $ scutil --set ComputerName new_hostname
    $ scutil --set LocalHostName new_hostname

I use my initials then some indicator of the machine type, like *ss-mbp17*
for my 17" MacBook Pro. 

Do the same for your Bonjour name...hit Fn+F6 to bring up the System
Preferences panel (didn't know you could do that with a keyboard
shortcut huh?), and type Sharing. Enter your new Computer Name here.

Keyboard Shortcut
-----------------
Setup option-command-, as a shortcut to launch System Preferences, since
you'll be doing it a lot in the future: http://goo.gl/CvEIA

Desktop
-------
* Check for updates
* Turn off Scroll direction:natural
* Dock on the left, remove unused icons
* Tweak keyboard repeat rate (key repeat=fast, delay=short)
* Turn on Firewall (Security & Preferences)

Chrome
------
Baller way to install:

    $ mkdir ~/tmp && cd tmp
    $ curl -O https://dl.google.com/chrome/mac/stable/GGRM/googlechrome.dmg 
    $ open googlechrome.dmg
    $ diskutil unmount "Google Chrome"
    $ rm googlechrome.dmg

Then: 

* Make it the default browser
* Add it to the dock, under Finder
* Install vimium extension (just Google for "vimium")

Other groovey extensions: 1Password, AdBlock, dotjs (remove), Google +1 Button,
    JoinTabs (optional), SessionBuddy, Syntaxtic

XCODE
-----
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

HOMEBREW
--------
Install brew before changing shells since it requires /bin/sh.

Instructions located at http://mxcl.github.com/homebrew/. MacPorts users
should read [why they should switch to brew][3].

[3]: http://lostincode.net/blog/homebrew

Do all this from Terminal.app; we'll swap out to iTerm2 later.

    $ ruby <(curl -fsSkL raw.github.com/mxcl/homebrew/go)
    $ brew doctor
    $ brew install cowsay
    $ rehash
    $ cowsay "brew install worked!"

You might need to do the following. I did not on a clean Mt. Lion build.
To satisfy the xcode-select "error" reported by brew doctor, see
https://github.com/mxcl/homebrew/issues/10245 and run

    $ sudo xcode-select -switch /usr/bin

/usr/local/bin vs /usr/bin in path
----------------------------------
Now that brew thinks it is working, try installing a utility that was
already installed by the Command Line Tools, namely "git" (and a few
other useful utilities).

    $ brew install git hub wget mercurial
    $ brew doctor

Although 'brew doctor' didn't report any problems in the previous step,
it probably does now. Basically, you need /usr/local/bin in your path
before /usr/bin. That's why the Apple-installed /usr/bin/git is being
run before /usr/local/bin/git, the version installed by brew (the most
up-to-date version).

This will be fixed shortly, when we use a new .cshrc file pulled from my
dotfiles.

Change default shell
--------------------
Change your shell from bash to tcsh (temporary).
    
    $ chsh -s /bin/tcsh

Log out, then back in.

Use my .dotfiles
----------------
Follow instructions at
[rm8t/dotfiles](http://github.com/rm8t/dotfiles). It uses tcsh (most
people are using zsh now, but I have old habits), so it installs a
.cshrc and a pretty custom .vimrc. 


Solarized
---------

    $ mkdir -p ~/projects/repos
    $ cd ~/projects/repos
    $ hub clone altercation/solarized

Then follow instructions under iterm2-colors-solarized folder to add the color
profiles to iTerm 2. I create two named profiles called "solarized-dark" and 
"solarized-light", each set to the corresponding color profile and Monoco 18pt.
Light is my default. Keep minimum color contrast set to low.

iTerm 2
-------

* Install [iTerm2](www.iTerm2.com) - beta version is fine.
* Add two new profiles: "solarized-light" and "solarized-dark".  
  Set the color contrast profile to *low*, otherwise colors will be
  washed out.
* Delete the default profile.
* Set both color schemes to the appropriate Solarized files from previous step.
* I use 18 pt. Menlo for font. Window: columns=100, rows=35


SSH for GitHub
--------------
You only need this step if you'll be pushing repos back to github, or if
you plan on using SSH anywhere. Instructions at [github][2].

[2]: (http://help.github.com/mac-set-up-git/

LESS
----
Brew does not install duplicates by default, so work with an alt fork

    $ brew tap homebrew/dupes
    $ brew install less 


RUBY
----
Lion comes with ruby 1.8.7. If you plan on using brew to install the
latest version (1.9.3 as of this writing), then it's easy to do.
However, I strongly recommend using RVM to manage multiple Ruby
installations.

Since this is more about using Node, I'll skip Ruby, RVM and any gems
for now. But if you go down that path, start with Step 6 at 
[Mouncef Belyamani's][10] tutorial.

[10]:
(http://www.moncefbelyamani.com/how-to-install-xcode-homebrew-git-rvm-ruby-on-mac/)

PYTHON (optional)
-----------------
Sooner or later, you will need python, another fine language.
Install as "framework" per http://docs.python-guide.org/en/latest/starting/install/osx/

    % brew install python --framework

todo: install pip
todo: maybe not install virtualenv 
    http://kev.inburke.com/kevin/virtualenv-is-an-anti-pattern-for-beginners/

VIM
---
Install new version of vim that enables system clipboard access for the
Mac. You'll need to edit the build formula for vim and add a single line
to the configuration section.

    $ brew edit vim

Now add the following line somewhere around line 30:

    "--enable-clipboard",

    $ brew install vim
    $ ln -s /usr/local/bin/vim /usr/local/bin/vi  # I still call it vi

TODO: I should submit the above change as a pull request to homebrew.

NODE
----
With the Command Line Tools-only option from earlier steps (i.e., you
don't install Xcode), I found that with Mountain Lion I had to first run
the following command before node would successfully build. 

    $ sudo xcode-select -switch /usr/bin

I get conflicting reports on how the above line should be handled when
you don't install XCode. 

    $ brew install node
    $ curl http://npmjs.org/install.sh | sh     # install npm

# Setup Perforce Visual Merge as gits visual tool (Mac OS only)
http://www.andymcintosh.com/?p=33
http://gitguru.com/2009/02/22/integrating-git-with-a-visual-merge-tool/
Install app from DMG. Add two 2 line sh scripts. Modify .gitconfig

GNU utilities TCSH
------------------
The 'ls' version built in to tcsh will display folders and files in
color when you use the flag "-G". But it sorts the folders along with
the files. I wanted the folders displayed first, then the files. Turns
out the GNU 'coreutils' package includes 'gls', that does just that.

But to enable color, it requires you to set a variable 'LS_COLOR' that
is strangely set by running another utility, gdircolors. And that
returns a string that is incompatible with tcsh LS_COLOR. 

The solution is to give gdircolors an initialization file, which is
pulled from a https://github.com/seebi/dircolors-solarized. Thank you
seebi for bringing Solarized colors to GNU utilities!

    $ brew install coreutils
    $ cd ~/projects/repo
    $ hub clone seebi/dircolors-solarized

Now edit your .cshrc to put the coreutils in the path, and to initialize
the LS_COLOR variable with the output of dircolors using the solarized
version. This is a lot of work to get color *and* sorting. Basically
tcsh is put together with so much string and tape. I'm guessing you
don't have to do this with zsh. 

    eval `gdircolors -c ~/projects/repos/dircolors-solarized/dircolors.ansi-universal`
    setenv LS_OPTIONS "--color=auto --group-directories-first -F"
    alias ls 'gls $LS_OPTIONS'



CloudApp (optional)
-------------------

Makes sharing screenshots and links trivial.
(Enable autoupload of screenshots)
SHIFT-COMMAND-4

Stop Here
=========

THE REST OF THIS IS UNTESTED.  IT IS HERE FOR FUTURE REFERENCE.  I PLAN
ON FIXING IT OR REMOVING IT EVENTUALLY.

Using node/express/jade
http://dandean.com/nodejs-npm-express-osx/

Then send it off to herokU

Install hub (teaches git about github)
http://defunkt.io/hub/

Consider using Makefile for .dotfiles like
https://github.com/domnikl/.home

To do:
move to sorin zsh
build .dotfiles with make
xcode update (reboot first)
look into janus
find a good screen shot app

Put something in on Markdown
http://dcurt.is/the-markdown-mark

Install *Marked* app from AppStore $3.99

note: removed macports crap with this:
sudo mv /opt/local tmp/macports

Comment .vimrc with this?
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

    $ ps -p $$      # see your current shell
    $ brew install zsh
    $ sudo chsh -s /usr/local/bin/zsh scott

# Setup oh-my-zsh

    $ hub clone sorin-ionescu/oh-my-zsh ~/.oh-my-zsh
    $ cd ~/.oh-my-zsh
    $ git submodule update --init --recursive
    $ cp cp templates/zshrc.zsh ~/.zshrc
    $ sudo mv /etc/zshenv /etc/profile
    $ logout
