Mac Config for Development
==========================

How I spend my first 15 minutes with a new macOS.

> Last tested on macOS Big Sur (Feb 2021)  
> Last tested on macOS Monterey 12.0.1 (Nov 2021) 

I'm a touch typist. I avoid the mouse whenever I can for speed. So some
of my configuration on the Mac is geared around that.

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
              Text                 × Capitalize words automatically
              Shortcuts            Mission Control → Turn Do Not Disturb On/Off: ⌘D 
              Shortcuts            Screenshots → Copy picture of selected area to clipboard: ⌘'
              Shortcuts†           App Shortcuts → [+] title: "System Preferences..." keys: ⌘⌥, 

trackpad      point & click        ✓ Tap to Click
              scroll & zoom        × Scroll direction: natural
              more gestures‡       ✓ Enable App Exposé

accessibility zoom                 ✓ Use scroll gesture with modifier keys to zoom (^ control)
              pointer control◊     trackpad options... ✓ enable dragging (three finger drag)

dock          -                    position on screen (left)§
              -                    ✓ Minimize windows into application icon
              -                    × Animate opening applications

sound         sound effects        Select an alert sound: Boop

spotlight     search results       × Siri suggestions

siri          -                    × Disable Ask Siri
```

Note:

† **⌘,** for app preferences; **⌥⌘,** for system preferences.  
‡ Why is this the only one disabled?
§ Preserves the more valuable vertical space for code, since width is plentiful in 16:9.
◊ Press control then two-finger swipe in/out on trackpad, it's [magic](https://discussions.apple.com/thread/6869616).

1.2 Finder Preferences
----------------------

Launch Finder and go to Preferences **⌘,**

Tab           | Option
:-------------|:---------
General       | ✓ Show (Hard disks & External Disks)
General       | New Finder windows shows: Desktop
Sidebar       | × AirDrop†
Sidebar       | ✓ (your home)
Sidebar       | ✓ Hard Disks
Sidebar       | × Tags
Advanced      | ✓ Show all filename extension‡ 
Advanced      | × Show warning before changing an extension◊
Advanced      | ✓ Remove items from trash after 30 days
Advanced      | ✓ Keep folders on top (both options)
Advanced      | When performing search (Search the current folder)

Note:

† AirDrop is accessible from the Finder "Go" menu.
‡ `CMD + SHIFT + .` will toggle hidden files on and off
◊ YOLO

Favorites order in the Finder pane (you can drag to re-order items):

1. Home
2. Desktop
3. Documents
4. Downloads
5. Dropbox | OneDrive
6. Applications

From the Finder menu: 

Menu       | Option
:----------|:---------
View       | as List
View       | Show Path Bar
View       | Show Status Bar


1.3 Messages Preferences
--------------------------

Launch `Messages`, hit `CMD-,` and change the Message received sound from `Note
(default)' to `Bamboo`. `Note` has an ADSR release of about 6 seconds: way too long.

1.4 Rename Your Computer
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

2. Apps
=======

Minimal set of command line and GUI apps.

2.1 Homebrew
------------

[Homebrew](http://brew.sh) is the App Store for the command line.

Instructions located at http://brew.sh

Do all this from Terminal.app; we'll swap out to iTerm2 or Alacritty later.
```bash
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

On Intel macOS, brew installs to '/usr/local/'.
On Apple Silicon (M1, M1 Pro and M1 Max) brew installs to '/opt/homebrew',
which is not in the path by default. So run these two steps:

```bash
    eval "$(/opt/homebrew/bin/brew shellenv)"
    echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
```

Install a few command line essentials (some are upgrades from macOS):

```bash
  brew install wget htop tree tmux neovim
```

macOS ships with these apps, but they're other old due to GPL v3 
licensing (bash), just old (less), or core that I want to easily update via
brew in the future (zsh, git)


```bash
  brew install bash     # 3.2.57 --> 5.1.8
  brew install git      # 2.30.1 --> 2.33.1
  brew install zsh      # same: 5.8 as of November 2021
  brew install less     # v487 --> v590
  rehash
```

I use these all the time. Most are written in Rust:
```bash
  brew install ascii hyperfine dust exa xsv ripgrep tokei httpie
```

Install my core Mac apps

```bash
  brew install xquartz rectangle marked
```

For programming, you're gonna need these:
```bash
  brew install llvm
```

Run Ubuntu in a VM, because why not?
```bash
  brew install --cask utm
  mkdir ~/tmp && cd ~/tmp
  wget https://cdimage.ubuntu.com/releases/20.04/release/ubuntu-20.04.3-live-server-arm64.iso
```


2.2 Rectangle
-------------

[Rectangle](https://github.com/rxhanson/Rectangle) is a keyboard-based
window movement tool, similar to Spectacle (retired).

Hit the gears and "Remove keyboard restrictions". Also, set Repeated commands to `cycle 1/2, 2/3 and 1/3 on
actions".

![gears](img/rectangle-gear.webp?raw=true "rectangle gears")

Configure the settings as shown in the screenshot below. I find it easiest to
just "x out" all the existing shortcuts and start over. You can ignore the stuff below the fold.

![config](img/rectangle-shortcuts.webp?raw=true "rectangle shortcuts")


https://github.com/rxhanson/Rectangle

Ensure it is set to Launch on login.

# 3. Programming

## 3.1 Gitconfig

Assuming you have a github.com account, tell your Mac about it. Follow
[these instructions](https://help.github.com/articles/set-up-git).

    $ git config --global user.name "your name here"
    $ git config --global user.email "your@email.com"
    $ git config --global credential.helper osxkeychain

## Python (via miniconda)
## Node (via NVM)

Don't install node directly; use the [node version
manager](https://github.com/nvm-sh/nvm).

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
```

## Programming Fonts

There's a big set of programmer-friendly, monospaced fonts that we can
use. They also have a handful of extra glyphs that make certain symbols
for git and Powerline (a vim and shell plugin) look nicer.

```bash
    brew tap homebrew/cask-fonts
    brew cask install font-inconsolatago-nerd-font        # clear 0 vs O
    brew cask install font-jetbrains-mono
```

Other good fonts:
> firacode  # Mozilla, many programming ligatures
> dejavu # Linux, ~3300 glyphs
> source code pro # Adobe, clear punctuation, many weights https://blog.typekit.com/2012/09/24/source-code-pro/
> noto mono # Google, available for 209 languages
> nerd fonts # https://www.nerdfonts.com/
> jetbrains mono: https://www.jetbrains.com/lp/mono/

> confusing characters to test:
```text
    1Il|iO0oB8
    <>^"^$\/()|?+*[]{},.
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
