# Mac Config for Web Development

> Tested for Yosemite  
> Last updated in January 2015

I just found http://nerderati.com/2014/09/03/installing-matplotlib-numpy-scipy-nltk-and-pandas-on-os-x-without-going-crazy/. Need to examine that for some updated tips.

The steps below assume you have a clean, fully patched Mac.

1. Update Computer
==================
Check for system updates in the **App Store**.

2. System Preferences
=====================

### Keyboard

Tab      | Option
---------|---------
Keyboard | Key Repeat: **fast**
         | Delay: **short**
         | Modifier Keys: Caps Lock => Control

Find the **Shortcuts** tab. Now since any application's preferences is ⌘-comma, 
I believe ⌥⌘-comma should display the _system_ preferences.

1. Select **App Shortcuts** from list on the left.
1. Hit the + button  
1. Type **System Preferences...** exactly, with the elipsis at the end
1. Use option+command+comma  

### Trackpad

Tab           | Option
--------------|---------
Point & Click | Enable **Tap to Click**
              | Enable **Three-finger drag** 
Scroll & Zoom | Disable **Scroll direction: natural**
More Gestures | Enable **App Exposé**

### Accessibility
Feature          | Option
-----------------|---------
Zoom             | Check **Use scroll gesture with modifier keys to zoom**
Mouse & Trakpad † | Trackpad Options... | Enable dragging **three finger drag**

† On the 2015 12" MacBook, the location for three-finger drag has changed. It's in the [hidden deep in Accessibility](https://discussions.apple.com/thread/6869616).


### Dock
Position on screen: Left  
Enable Minimize windows into application icon  
Disable Animate opening applications  

### Accessibility
Select Zoom  
Enable "Use scroll gesture with modifier keys to zoom"  

### Security & Privacy
Tab           | Option
--------------|---------
General       | Allow apps downloaded from **Anywhere** (NB: security risk)
Firewall      | Turn on the Firewall

3. Finder 
==========
Finder Preferences ⌘,

Tab           | Option
--------------|---------
Advanced      | Enable **Show all filename extensions**
Sidebar       | Disable **All My Files** 
              | Disable **AirDrop**. 
              | Enable **(your home)** and drag to the top in the finder menu
              | Enable **Hard Disks**

> AirDrop and AllMyFiles are accessible from the Finder "Go" menu.  
> They're used so infrequently to deserve a top spot.

4. Chrome
=========
Yes, you can install Chrome from command line. 

    $ mkdir ~/tmp && cd tmp
    $ curl -O https://dl.google.com/chrome/mac/stable/GGRM/googlechrome.dmg 
    $ open googlechrome.dmg
    $ diskutil unmount "Google Chrome"
    $ rm googlechrome.dmg

Then:

1. Launch Chrome. It'll ask if it can be your default browser.
2. Right-click on it's icon, select Options | Keep in dock.

Optional:
* Install vimium extension (just Google for "vimium")


6. Rename Computer
==================
Open `Terminal.app`.

By default, your computer probably has a name like John Smith's
Computer. Rename it easily from Terminal:

I use my initials then some indicator of the machine type, like *ss-mbp15*
for my 15" MacBook Pro.

    $ sudo scutil --set HostName ss-mbp15
    $ sudo scutil --set ComputerName ss-mbp15
    $ sudo scutil --set LocalHostName ss-mbp15

7. Fix the Path
===============

> I don't think you need to do this step in Yosemite!

We own everything under /usr/local; Apple (and Unix in general) doesn't
put anything in here. In order for our apps to superseed included
utilities like `git`, we need /usr/local/bin before /usr/bin in path.

Not sure if this works, but ensure `/usr/local/bin` is at the top of
`/etc/paths`

    $ echo "/usr/local/bin" | cat - /etc/paths | sudo tee /etc/paths 

Again, close this session `CTRL-D` and restart a new one.


8. HOMEBREW
===========
[Homebrew](http://brew.sh) is the App Store for the command line. 

> Important: if you haven't already installed the Apple development tools
> `brew` will detect this and prompt you to do so. Install the command
> line tools, not the entire Xcode.

Instructions located at http://mxcl.github.com/homebrew/. MacPorts users
should read [why they should switch to brew](http://lostincode.net/blog/homebrew).

Do all this from Terminal.app; we'll swap out to iTerm2 later.
    
    $ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    $ brew doctor
    $ brew install cowsay
    $ cowsay "brew works!"

Now that brew thinks it is working, try installing a utility that was
already installed by the Command Line Tools, namely "git" (and a few
other useful utilities).

    $ brew install git wget gist htop source-highlight hub tree node ack httpie p7zip
    $ brew install openssl && brew link openssl --force		
    $ brew doctor

We'll use the Cask extension for Homebrew to install some Mac apps

    $ brew install caskroom/cask/brew-cask
    $ brew cask install atom github spectacle iterm2 

One of the Mac apps, [Atom](http://atom.io) is a new, hackable text editor. It
has rudimentary vi(m) support, which I like. Here are a few plug-ins to make it noice.

    $ apm install language-jade vim-mode minimap file-icons 
    $ apm install open-in-github-app terminal-status
    
5. Spectacle App
================
> Note: you installed this in the previous step. Run it.

Wrangling windows is like hearding cats: no matter what you do, they still get 
away. Microsoft Windows has a great, built-in solution: Windows + arrow keys. 

[Spectacle](http://spectacleapp.com/) is the best (and free) solution for the 
Mac. I've tried about a half-dozen of them. Some of the defaults conflict with 
Chrome's shortcuts to move between tabs, so I've remapped mine to something 
sane. Check out the screenshot below for mine:

![Spectacle Preferences](http://f.cl.ly/items/1q0x3g2N3t1u1H091J0b/spectacle.png)

Let Spectacle do it's thing by enabling it in **System Preferences -> Security -> Accessibility**

Also, once launched, click on it's icon, go to preferenes and enable **Launch Spectacle at login**

9. Setup Git for github
========================
Assuming you have a github.com account, tell your Mac about it. Follow
[these instructions](https://help.github.com/articles/set-up-git).

    $ git config --global user.name "your name here"
    $ git config --global user.email "your@email.com"
    $ git config --global credential.helper osxkeychain

10. Solarized
============
Solarized is a well-known common color scheme that works across vim,
iTerm, my custom prompt and the GNU utilities.

    $ mkdir ~/lib && cd ~/lib
    $ hub clone altercation/solarized

Then follow instructions under iterm2-colors-solarized folder to add the color
profiles to iTerm 2. I create two named profiles called "solarized-dark" and 
"solarized-light", each set to the corresponding color profile and Monoco 18pt.
Light is my default. Keep minimum color contrast set to low.

11. Install Programming Fonts
=============================
There's a big set of programmer-friendly, monospaced fonts that we can
use. They also have a handful of extra glyphs that make certain symbols
for git and Powerline (a vim and shell plugin) look nicer. 

	$ cd ~/lib
	$ hub clone Lokaltog/powerline-fonts

Launch the Font Book app. Create a new collection called
**Powerline**. Drag the powerline-fonts folder into the empty space.
This action registers the typefaces with the Mac and allow you to choose one 
when you configure the fonts in iTerm, below.


12. iTerm 
==========

iTerm is a full-featured replacement for the anemic Terminal.app that
ships with the Mac. It's also referred to as iTerm 2.

* Download the iTerm2 **Test** release or the **Stable release if it's v2** from 
  [iTerm2 downloads](http://www.iterm2.com/#/section/downloads).
* Open the zip to extract 'iTerm.app', then drag it to your Applications
  folder.
* Run the app (it should be available now from Spotlight).
* Right click on the icon and choose Options | Keep in Dock

Setup iTerm

* Add two new profiles: "solarized-light" and "solarized-dark".  
  Set the color contrast profile to *low*, otherwise colors will be
  washed out.
* Delete the **default** profile.
* Set both color schemes to the appropriate Solarized files from previous step.
* I use 18 pt. Menlo for font. Window: columns=100, rows=35
* Use the same font for both 'Regular Font' and 'Non-ASCII Font'

When finished, your profile seetings dialog box should look like the one
in this [github comment](https://github.com/scottstanfield/newmac/issues/2).

13. Change default shell
========================
Launch Terminal. Now change your shell from `bash` to `csh`

    $ chsh -s /bin/csh

Close this session `CTRL-D` and start a new one.

14. Use my .dotfiles
====================
Follow instructions at
[scottstanfield/dotfiles](http://github.com/scottstanfield/dotfiles). It uses tcsh (most
people are using zsh now, but I have old habits), so it installs a
.cshrc and a pretty custom .vimrc.

15. A better 'ls'
======================
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

> Note: consider mkdir ~/lib and clone this repo into there

    $ cd ~/lib
    $ hub clone seebi/dircolors-solarized

Now edit your .cshrc to put the coreutils in the path, and to initialize
the LS_COLOR variable with the output of dircolors using the solarized
version. This is a lot of work to get color *and* sorting. Basically
tcsh is put together with so much string and tape. I'm guessing you
don't have to do this with zsh. 

    eval `gdircolors -c ~/lib/dircolors-solarized/dircolors.ansi-universal`
    setenv LS_OPTIONS "--color=auto --group-directories-first -F"
    alias ls 'gls $LS_OPTIONS'

Log out, then back in.

17. LESS (optional)
===================
`less` is a terminal pager program used to look at text files. It's
similar to `more` but improved, allowing both forward and backwards
navigation in a file. 

Version 418 comes with Mt Lion, but there are features in the latest
stable (451) that are nice to have.

But brew does not install system duplicates by default, so work with an alt fork.

    $ brew tap homebrew/dupes
    $ brew install less 


# Extras

Formd
=====
There's a great Python command-line tool called [formd](http://drbunsen.github.io/formd/) that keeps 
Markdown links looking clean. Notice how all my links are at the bottom? That's
`formd` in action.


Handy Keyboard Shortcuts
========================
Readline / EMACs

    ctrl+a  beginning of line  
    ctrl+e  end of line  
    ctrl+w  delete backwards by word  

    Cmd + Space Spotlight
    Cmd + Tab   Just like Alt-Tab in Windows
    Cmd + +/-   Most apps make text bigger or smaller
    Cmd + H     Hide or Minimize

TextEdit
========
This is your basic text editor. For some reason, it defaults to
RichText, which is stupid. Launch, display its Preferences dialog and
change:

    Format to Plain text
    Plain text font to something larger if you want
    Turn off *all* the Options

Reference: http://zsh.sourceforge.net/Guide/zshguide03.html


Good Mac Apps to Have
=====================

[MacDown](http://macdown.uranusjr.com/) is an open-source Markdown
editor that handles Github-flavored Markdown (GFM) nicely.

[Dash](http://kapeli.com/dash) gives you offline access to 150+ doc sets like vim, markdown, css, html, python,
etc.


Github
======
https://help.github.com/articles/set-up-git

git config --global user.name "your name here"
git config --global user.email "you@example.com"
git config --global credential.helper osxkeychain
git config --global push.default simple

ZSH config
==========
instead of rehash, try setopt nohashdirs

There's some weirdness with the way Apple setup the zsh config files.
Read more here: https://github.com/sorin-ionescu/prezto/issues/381
Fix it with the following command

$ sudo mv /etc/{zshenv,zprofile}
    
# Inspired from

* My collegue [Matt Carrier](https://github.com/icecreammatt) and his excellent dotfiles
* [Thoughtbot's](https://github.com/thoughtbot/laptop/blob/master/mac) laptop setup.
* Mark H. Nichols excellent writeup on [configuring ZSH from scratch](http://zanshin.net/2013/02/02/zsh-configuration-from-the-ground-up/)
* [zshuery](https://github.com/myfreeweb/zshuery/blob/master/zshuery.sh), 
  a one file good .zshrc config
* Moncef Belyamani has a [guide for Mavericks](http://bit.ly/VQsHy1).
* And finally, [Top Eight OSX Utilites Developers Should Know](http://www.mitchchn.me/2014/os-x-terminal/)
