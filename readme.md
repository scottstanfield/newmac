Mac Config for Development
==========================

How I spend my first 15 minutes with a new macOS.

> Last tested on macOS Mojave (Dec 2018)

I'm a touch typist. I avoid the mouse whenever I can for speed. So some
of my configuration on the Mac is geared around that.

## 1. System Preferences

My three critical modifications:

1. Key repeat fast with little delay
2. Scroll direction: [unnatural](https://www.lifewire.com/how-to-change-scrolling-direction-on-mac-2260835)
3. Mapping CAPS LOCK to CONTROL, cause [vim](http://xahlee.info/kbd/ADM-3A_terminal.html), [readline](https://spin.atomicobject.com/2017/11/10/readline-productivity/), and it's useless.

![map caps to control](img/caps-lock-mapping.png?raw=true "Map CAPS to CONTROL")

Hit the Apple menu, click System Preferences...and have at it:

```text
keyboard      keyboard             key repeat (fast)
              keyboard             delay until repeat (short)
              keyboard             modifier keys...     Caps Lock ⇪ (^ Control)
              shortcuts            App Shortcuts --> [+] title: "System Preferences..." keys: ⌘ ⌥ ,	    
trackpad      point & click        ✓ Tap to Click
              point & click        ✓ Three-finger drag

              point & click†       ✓ Silent clicking
	      
              scroll & zoom        × Scroll direction: natural
              more gestures        ✓ Enable App Exposé
accessibility zoom                 ✓ Use scroll gesture with modifier keys to zoom (^ control)
              pointer control‡     trackpad options... ✓ enable dragging (three finger drag)
dock          -                    position on screen (left)
              -                    ✓ minimize windows into application icon
              -                    × disable animate opening applications
sound         sound effects        ✓ show volume in menu bar
spotlight     search results       × spotlight suggestions
```

**Notes**
\* Since app preferences are ⌘-comma, I like the symmetry of ⌥⌘-comma for system preferences.  
† Added in macOS Mojave (v10.14).  
‡ Hold down control and zoom in/out with the mouse wheel, it's [magic](https://discussions.apple.com/thread/6869616).

Other personal preferences: 
* disable Siri



## 2. Finder 

Launch Finder and go to Preferences (⌘-comma)

Tab           | Option
--------------|---------
General       | ✓ Show (Hard disks & External Disks)
Sidebar       | × All My Files†
Sidebar       | × AirDrop
Sidebar       | ✓ (your home) and drag to the top in the finder menu
Sidebar       | ✓ Hard Disks
Advanced      | ✓ Show all filename extensions\*
Advanced      | ✓ Keep folders on top
Advanced      | When performing search (Search the current folder)

**Notes**
* `CMD + SHIFT + .` will toggle hidden files on and off
† AirDrop and AllMyFiles are accessible from the Finder "Go" menu. They're used too infrequently to deserve a top spot.

My order in the Finder pane (you can drag to re-order items):

1. Home
2. Desktop
3. Documents
4. Downloads
5. Dropbox | OneDrive
6. Applications

## 3. Rename Computer

Open `Terminal.app`.

By default, your computer probably has a name like `Dutch Morgan's Computer`. Rename it easily from Terminal:

I use my initials then some indicator of the machine type, like *ss-mbp15* for my 15" MacBook Pro.

```bash
  sudo scutil --set HostName ss-mbp15
  ^Host^Computer
  ^Computer^LocalHost
```

## 4. HOMEBREW

[Homebrew](http://brew.sh) is the App Store for the command line. 

> Important: if you haven't already installed the Apple development tools
> `brew` will detect this and prompt you to do so. Install the command
> line tools, not the entire Xcode.

Instructions located at http://brew.sh 

Do all this from Terminal.app; we'll swap out to iTerm2 later.
    
```bash
    $ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    $ brew doctor
```

Now that brew thinks it is working, try installing a utility that was
already installed by the Command Line Tools, namely "git" (and a few
other useful utilities).

```bash
  brew install git wget cask htop tree httpie ripgrep tmux
  brew doctor
```

We'll use the Cask extension for Homebrew to install some Mac apps

```bash
  brew cask install google-chrome iterm2 spectacle visual-studio-code
```

## 5. Spectacle

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

## 6. Git

Assuming you have a github.com account, tell your Mac about it. Follow
[these instructions](https://help.github.com/articles/set-up-git).

    $ git config --global user.name "your name here"
    $ git config --global user.email "your@email.com"
    $ git config --global credential.helper osxkeychain

## 7. Solarized

Solarized is a well-known common color scheme that works across vim,
iTerm, my custom prompt and the GNU utilities.

    $ mkdir ~/lib && cd ~/lib
    $ hub clone altercation/solarized

Then follow instructions under iterm2-colors-solarized folder to add the color
profiles to iTerm 2. I create two named profiles called "solarized-dark" and 
"solarized-light", each set to the corresponding color profile and Monoco 18pt.
Light is my default. Keep minimum color contrast set to low.

## 8. Programming Fonts

There's a big set of programmer-friendly, monospaced fonts that we can
use. They also have a handful of extra glyphs that make certain symbols
for git and Powerline (a vim and shell plugin) look nicer. 

	$ cd ~/lib
	$ hub clone Lokaltog/powerline-fonts

Launch the Font Book app. Create a new collection called
**Powerline**. Drag the powerline-fonts folder into the empty space.
This action registers the typefaces with the Mac and allow you to choose one 
when you configure the fonts in iTerm, below.


## 9. iTerm2

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

## Change default shell
Launch Terminal. Now change your shell from `bash` to `csh`

    $ chsh -s /bin/csh

Close this session `CTRL-D` and start a new one.

##  Use my .dotfiles

Follow instructions at
[scottstanfield/dotfiles](http://github.com/scottstanfield/dotfiles). It uses tcsh (most
people are using zsh now, but I have old habits), so it installs a
.cshrc and a pretty custom .vimrc.

## A better 'ls'

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

##  LESS (optional)

`less` is a terminal pager program used to look at text files. It's
similar to `more` but improved, allowing both forward and backwards
navigation in a file. 

Version 418 comes with Mt Lion, but there are features in the latest
stable (451) that are nice to have.

But brew does not install system duplicates by default, so work with an alt fork.

    $ brew tap homebrew/dupes
    $ brew install less 


# Extras

Python
------

[numpy-pandas-python](http://nerderati.com/2014/09/03/installing-matplotlib-numpy-scipy-nltk-and-pandas-on-os-x-without-going-crazy). Need to examine that for some updated tips.


Formd
-----
There's a great Python command-line tool called [formd](http://drbunsen.github.io/formd/) that keeps 
Markdown links looking clean. Notice how all my links are at the bottom? That's
`formd` in action.


Handy Keyboard Shortcuts
------------------------
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

Python
======
[numpy-pandas-python](http://nerderati.com/2014/09/03/installing-matplotlib-numpy-scipy-nltk-and-pandas-on-os-x-without-going-crazy). Need to examine that for some updated tips.


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

### image processing 

I'm using Monosnap to capture and annote the screenshots. Then
ImageMagik for a smart rescale down to 75% size:

```
mogrify -path img -filter spline -resize 75% -unsharp 0x0.75+0.75+0.008 ~/Pictures/Monosnap/*.png
```
Finally `ImageOptim` to compress down to virtually nothing (Lossy with PNGCrush)
