Mac Config for Development
==========================

How I spend my first 15 minutes with a new macOS.

> Last tested on macOS Big Sur (Feb 2021)
> I've been working on this document for 11 years.

I'm a touch typist. I avoid the mouse whenever I can for speed. So some
of my configuration on the Mac is geared around that.

>> Renamed master --> main: steps to update on your local clone
```
git branch -m master main
git fetch origin
git branch -u origin/main main
git remote set-head origin -a
```

1.1 System Preferences
----------------------

My three critical modifications:

1. Mapping CAPS LOCK to CONTROL, because: [vim](http://xahlee.info/kbd/ADM-3A_terminal.html), [readline](https://spin.atomicobject.com/2017/11/10/readline-productivity/), and it's useless
2. Scroll direction: [unnatural](https://www.lifewire.com/how-to-change-scrolling-direction-on-mac-2260835)
3. Key repeat fast, with little delay

![map caps to control](img/caps-lock-mapping.png?raw=true "Map CAPS to CONTROL")

Hit the Apple menu, click System Preferences...and have at it:

```text
Keyboard      Keyboard             Key Repeat → fast
              Keyboard             Delay Until Repeat → short
              Keyboard             [Modifier Keys...]  Caps Lock ⇪ Key: ^ Control
              Shortcuts†           App Shortcuts → [+] title: "System Preferences..." keys: ⌘⌥,

trackpad      point & click        ✓ Tap to Click
              point & click‡       ✓ Silent clicking
              scroll & zoom        × Scroll direction: natural
              more gestures        ✓ Enable App Exposé

accessibility zoom                 ✓ Use scroll gesture with modifier keys to zoom (^ control)
              pointer control◊     trackpad options... ✓ enable dragging (three finger drag)

dock          -                    position on screen (left)
              -                    ✓ Minimize windows into application icon
              -                    × Animate opening applications

sound         sound effects        ✓ show volume in menu bar
              sound effects        Select an alert sound: Boop

spotlight     search results       × Siri suggestions

siri          -                    × Disable Ask Siri
```

Note:

† **⌘,** for app preferences; **⌥⌘,** for system preferences.  
‡ Added in macOS Mojave (v10.14).  
◊ Hold down control and zoom in/out with the mouse wheel, it's [magic](https://discussions.apple.com/thread/6869616).

1.2 Finder Preferences
----------------------

Launch Finder and go to Preferences **⌘,**

Tab           | Option
:-------------|:---------
General       | ✓ Show (Hard disks & External Disks)
Sidebar       | × All My Files†
Sidebar       | × AirDrop
Sidebar       | ✓ (your home) and drag to the top in the finder menu
Sidebar       | ✓ Hard Disks
Advanced      | ✓ Show all filename extension‡ 
Advanced      | ✓ Keep folders on top
Advanced      | When performing search (Search the current folder)

Note:

† AirDrop and AllMyFiles are accessible from the Finder "Go" menu. They're used too infrequently to deserve a top spot.
‡ `CMD + SHIFT + .` will toggle hidden files on and off

Favorites order in the Finder pane (you can drag to re-order items):

1. Home
2. Desktop
3. Documents
4. Downloads
5. Dropbox | OneDrive
6. Applications

From the menu `View | Show Path Bar` and `View | Show Status Bar`.

1.3 Rename Your Computer
------------------------

Open `Terminal.app`.

By default, your computer probably has a name like `Dutch Morgan's
Computer`. Rename it easily from Terminal using
[scutil](https://ss64.com/osx/scutil.html)

I use my initials then some indicator of the machine type, like `ss-mbp15` for my 15" MacBook Pro.

```bash
  sudo scutil --set HostName ss-mbp15
  sudo scutil --set ComputerName ss-mbp15
  sudo scutil --set LocalHostName ss-mbp15
```

2.1 Homebrew
------------

[Homebrew](http://brew.sh) is the App Store for the command line. Instructions located at http://brew.sh

Do all this from Terminal.app; we'll swap out to iTerm2 later.
```bash
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

```bash
  path=(/opt/homebrew/bin $path[@])
  brew install bash git coreutils jq wget htop tree tmux neovim zsh less
```

My extra utilities:
```bash
  brew install ascii hyperfine dust exa xsv ripgrep tokei httpie fzf fd
```

We'll use the Cask extension for Homebrew to install some Mac apps

```bash
  brew install xquartz rectangle alacritty marked
```

2.2 Rectangle
-------------

[Rectangle](https://github.com/rxhanson/Rectangle) is a keyboard-based, window tiling app. It's MIT licenced and
installable from https://github.com/rxhanson/Rectangle/releases (look
for `RectangleX.XX.dmg`). Or with `brew`:

```
brew install rectangle
```

Configure the settings as shown below. I don't care for the defaults. My
thinking is: just mash all three of the keys in the lower left, then use
arrows to throw the windows around.

![config](img/rectangle-config.png?raw=true "rectangle config")

Home page: https://github.com/rxhanson/Rectangle

Ensure it is set to Launch on login.

# 3. Programming

## 3.1 Gitconfig

Assuming you have a github.com account, tell your Mac about it. Follow
[these instructions](https://help.github.com/articles/set-up-git).

    $ git config --global user.name "your name here"
    $ git config --global user.email "your@email.com"
    $ git config --global credential.helper osxkeychain

## Programming Fonts

I use Meslo with the Nerd Font icon patch.

```bash
    brew tap homebrew/cask-fonts
    brew install font-meslo-lg-nerd-font font-jetbrains-mono-nerd-font
```

## Alacritty

Use alacritty instead of iTerm2 or Terminal. It's configured in a single
file: ~/.alacritty.yml. Easy to version control, it's fast and
cross-platform.

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

# Extras

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
