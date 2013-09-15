Scott Stanfield  
Last updated: April 2013

---

Mac Config for Web Development
==============================
Tested for *Mountain Lion* (OSX 10.8).

The steps below assume you have a clean, fully patched Mac with Mt Lion (10.8.x).

This guide was inspired by Mr Belyamani's 
[Ruby focused guide](http://bit.ly/VQsHy1).

[Another guide](http://johanbrook.com/development/web-dev-environment/)
from John Brook.

0. Unpack the Computer
---------------------
This step was probably pretty obvious.


1. Rename Computer
------------------

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

2. Keyboard Shortcut
--------------------

Setup option-command-, as a shortcut to launch System Preferences, since
you'll be doing it a lot in the future: http://goo.gl/CvElA

3. Desktop
----------

* Check for updates
* Turn off Scroll direction:natural
* Dock on the left, remove unused icons
* Tweak keyboard repeat rate (key repeat=fast, delay=short)
* Turn on Firewall (Security & Preferences)

4. Chrome
---------

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

Other groovy extensions: 1Password, AdBlock, dotjs (remove), Google +1 Button,
    JoinTabs (optional), SessionBuddy, Syntaxtic

5. XCODE
--------
Note: for Mountain Lion, you may need to follow some extra steps. The
version of XCode is now 4.4. Read this first: https://gist.github.com/1860902

Most of the homebrew packages need the compiler (and header and libraries)
previously only available after a full 4GB Xcode install from AppStore, then
choosing the optional Command Line Tools from the preference pane. 

But Apple now allows the installation of the Command Line Tools
*without* needing to install Xcode. However you need a free Apple ID to
download the CLT. After that, head to [Apple Developer Tools][1] to
download. 

[1]: https://developer.apple.com/downloads/index.action

As of this writing, Command Line Tools for Xcode - Late March 2012 is current.

Note: If you have Xcode already and want to remove, uninstall with:

    $ sudo /Developer/Library/uninstall-devtools --mode=all

Full story at http://kennethreitz.com/xcode-gcc-and-homebrew.html

6. HOMEBREW
-----------
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

7. /usr/local/bin before /usr/bin in path
-----------------------------------------
Now that brew thinks it is working, try installing a utility that was
already installed by the Command Line Tools, namely "git" (and a few
other useful utilities).

    $ brew install git wget gist
    $ brew doctor

Although 'brew doctor' didn't report any problems in the previous step,
it probably does now. Basically, you need /usr/local/bin in your path
before /usr/bin. That's why the Apple-installed /usr/bin/git is being
run before /usr/local/bin/git, the version installed by brew (the most
up-to-date version).

This will be fixed shortly, when we use a new .cshrc file pulled from my
dotfiles.

8. Change default shell
-----------------------
Change your shell from bash to tcsh (temporary).
    
    $ chsh -s /bin/tcsh

9. GNU utilities TCSH
---------------------
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
    $ git clone git://github.com/seebi/dircolors-solarized.git

Now edit your .cshrc to put the coreutils in the path, and to initialize
the LS_COLOR variable with the output of dircolors using the solarized
version. This is a lot of work to get color *and* sorting. Basically
tcsh is put together with so much string and tape. I'm guessing you
don't have to do this with zsh. 

    eval `gdircolors -c ~/projects/repos/dircolors-solarized/dircolors.ansi-universal`
    setenv LS_OPTIONS "--color=auto --group-directories-first -F"
    alias ls 'gls $LS_OPTIONS'

Log out, then back in.

10. Use my .dotfiles
--------------------
Follow instructions at
[scottstanfield/dotfiles](http://github.com/scottstanfield/dotfiles). It uses tcsh (most
people are using zsh now, but I have old habits), so it installs a
.cshrc and a pretty custom .vimrc. 


11. Solarized
-------------
Solarized is a well-known common color scheme that works across vim,
iTerm, my custom prompt and the GNU utilities.

    $ mkdir -p ~/projects/repos 
    $ cd ~/projects/repos
    $ git clone git://github.com/altercation/solarized.git

Then follow instructions under iterm2-colors-solarized folder to add the color
profiles to iTerm 2. I create two named profiles called "solarized-dark" and 
"solarized-light", each set to the corresponding color profile and Monoco 18pt.
Light is my default. Keep minimum color contrast set to low.

12. iTerm 
----------

iTerm is a full-featured replacement for the anemic Terminal.app that
ships with the Mac. It's also referred to as iTerm 2.

* Download the iTerm2 beta (20130319 build or later) from [iTerm2
  downloads](https://code.google.com/p/iterm2/downloads/list?q=label:Featured).
* Open the zip to extract 'iTerm.app', then drag it to your Applications
  folder.
* Run the app (it should be available now from Spotlight).
* Right click on the icon and choose Options | Keep in Dock

If you installed my .dotfiles, you have the preference file already.
Bring up the preferences for iTerm and enter `~/.dotfiles/lib` for the
option at the bottom "Load preferences from a custom folder".

Otherwise, do it by hand:

* Add two new profiles: "solarized-light" and "solarized-dark".  
  Set the color contrast profile to *low*, otherwise colors will be
  washed out.
* Delete the default profile.
* Set both color schemes to the appropriate Solarized files from previous step.
* I use 18 pt. Menlo for font. Window: columns=100, rows=35
* Use the same font for both 'Regular Font' and 'Non-ASCII Font'

When finished, your profile seetings dialog box should look like the one
in this [github comment](https://github.com/scottstanfield/newmac/issues/2).

13. Setup Git for github
------------------------
Assuming you have a github.com account, tell your Mac about it. Follow
[these instructions](https://help.github.com/articles/set-up-git).

	$ git config --global user.name "your name here"
	$ git config --global user.emal "your@email.com"
	$ git config --global credential.helper osxkeychain

When you clone a repo, use the HTTPS URL instead of SSH. If you must use
the SSH repo, then follow [this guide](2) to setup your SSH keys.

[2]: (https://help.github.com/articles/generating-ssh-keys)

14. NODE
--------
With the Command Line Tools-only option from earlier steps (i.e., you
don't install Xcode), I found that with Mountain Lion I had to first run
the following command before node would successfully build. 

    $ sudo xcode-select -switch /usr/bin

I get conflicting reports on how the above line should be handled when
you don't install XCode. 

Node (as of version 0.8?) will install npm for you.

    $ brew install node
	$ npm install grunt-cli -g
	$ npm install nws -g

15. LESS (optional)
-------------------
`less` is a terminal pager program used to look at text files. It's
similar to `more` but improved, allowing both forward and backwards
navigation in a file. 

Version 418 comes with Mt Lion, but there are features in the latest
stable (451) that are nice to have.

But brew does not install system duplicates by default, so work with an alt fork.

    $ brew tap homebrew/dupes
    $ brew install less 


16. VIM (optional)
------------------
Install new version of vim that enables system *clipboard access* for the
Mac. You'll need to edit the build formula for vim and add a single line
to the configuration section.

    $ brew edit vim

Now add the following line somewhere around line 30:
Find the code around line 35 that looks like this:

`
	opts = language_opts
`

Add this line below:

`
	opts << "--enable-clipboard"
`

    $ brew install vim
    $ ln -s /usr/local/bin/vim /usr/local/bin/vi  # I still call it vi

TODO: I should submit the above change as a pull request to homebrew.

17. X11 (optional)
------------------
This [gist](https://gist.github.com/1860902) will help you get X11
working with Mountain Lion since it's no longer installed with OS X. 


