# Mac Config for Web Development

> Tested for Mavericks OS 10.9.2  
> Last updated on May 12, 2014
> THIS IS A DRAFT FOR MAVERICKS. AND IT'S CURRENTLY INCOMPLETE.

The steps below assume you have a clean, fully patched Mac.

Ideas liberally stolen from:

* [Thoughtbot's][1] laptop setup.
* Mark H. Nichols excellent writeup on [configuring ZSH from scratch][2]
* [zshuery][3], 
  a one file good .zshrc config
* Moncef Belyamani has a [guide for Mavericks][4].
* And finally, [Top Eight OSX Utilites Developers Should Know][5]

TEMPORARY
=========
Figure out how to explain steps to install the python library
[formd][6]. Provides nice Markdown
hyperlink management. Notice how all my links are at the bottom? That's
`formd` in action.

Keyboard Shortcuts you should know and love
================
Readline / EMACs

ctrl+a	beginning of line
ctrl+e 	end of line
ctrl+w	delete backwards by word

Cmd + Space	Spotlight
Cmd + Tab	Just like Alt-Tab in Windows
Cmd + +/-	Most apps make text bigger or smaller
Cmd + H		Hide or Minimize

TextEdit
=======
This is your basic text editor. For some reason, it defaults to
RichText, which is stupid. Launch, display its Preferences dialog and
change:

	Format to Plain text
	Plain text font to something larger if you want
	Turn off *all* the Options

System Preferences
==================
You're going to be bringing up this dialog a lot. Since preferences for
any application is cmd-comma, I was thinking option-command-comma should
map to overall system preferences.

## Keyboard/Keyboard
Key Repeat: fast
Delay Until Repeat: short
Modifier Keys...: Caps Lock Key => Control

## Keyboard/Shortcuts
Select "App Shortcuts"
Hit the + button
Type "System Preferences..." exactly
Use option+command+comma

## Trackpad/Point & Click
Enable Tap to click and Three finger drag

## Trackpad/Scroll & Zoom
Disable "Scroll direction: natural"

## Trackpad/More Gestures
Enable Mission Control (because why note?

## Dock
Position on screen: Left

## Desktop/Screen Saver

--> turn on zoom magnification with control+track padbash

Finder Preferences
==================
(In Sidebar)

Turn off All My Files and AirDrop. Both are accessible from the Finder "Go" menu. They're used so infrequently to deserve a top spot.

Turn on your home folder. I drag mine to the top of the list.

Turn on Hard disks.

(In Advanced)
Turn on Shall all filename extensions

Spectacle App
=============
Wrangling windows is like hearding cats: no matter what you do, they still get away. Microsoft Windows has a great, built-in solution: Windows + arrow keys. 

Spectacle is the best (and free) solution for the Mac. I've tried about a half-dozen of them. The defaults all all wrong, so you'll need to remap them.

http://spectacleapp.com/

Brew
====
brew install tree

after brew install:
$ rehash

Reference: http://zsh.sourceforge.net/Guide/zshguide03.html



Github
======
https://help.github.com/articles/set-up-git

git config --global user.name "your name here"
git config --global user.email "you@example.com"
git config --global credential.helper osxkeychain
git config --global push.default simple


Notes
=====
Keyboard shortcut first
Move rename computer down to later

ZSH config
==========
instead of rehash, try setopt nohashdirs



TEMPORARY END
=============


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
you'll be doing it a lot in the future: http://goo.gl/CvElA

Desktop
-------

* Check for updates
* Map CAPSLOCK to CTRL
* Turn off Scroll direction:natural
* Dock on the left, remove unused icons
* Tweak keyboard repeat rate (key repeat=fast, delay=short)
* Turn on Firewall (Security & Preferences)

Chrome
---------
Yes, you can install Chrome from command line. 

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
    JoinTabs (optional), SessionBuddy, Syntaxtic, LiveReload

Change default shell
-----------------------
Launch Terminal. Now change your shell from `bash` to `zsh`

    $ chsh -s /bin/zsh

I thought about using `brew` to install a new zsh, but the version
included with the Mac is only one version behind (two were redacted).

Close this session `CTRL-D` and start a new one.

Fix the Path
------------
We own everything under /usr/local; Apple (and Unix in general) doesn't
put anything in here. In order for our apps to superseed included
utilities like `git`, we need /usr/local/bin before /usr/bin in path.

Not sure if this works, but ensure `/usr/local/bin` is at the top of
`/etc/paths`

    $ echo "/usr/local/bin" | cat - /etc/paths | sudo tee /etc/paths 

There's some weirdness with the way Apple setup the zsh config files.
Read more here: https://github.com/sorin-ionescu/prezto/issues/381
Fix it with the following command

    $ sudo mv /etc/{zshenv,zprofile}

Again, close this session `CTRL-D` and restart a new one.


Command Line Tools
------------------
Apple doesn't ship a compiler with Mavericks for some reason. So you
need to trigger the installation by trying to run the compiler directly.
We need `gcc` installed so we can build the rest of our tools via
`brew` later.

Trigger installation of Command Line Tools by launching Terminal.app and
typing this

    $ gcc

Instructions are confusing. Just click the Install button. After install, verify:

    $ xcode-select -p
    /Library/Developer/CommandLineTools

And now verify that gcc is actually installed

    $ gcc --version


HOMEBREW
-----------
Install [brew][7] before changing shells since it requires /bin/sh.

Instructions located at http://mxcl.github.com/homebrew/. MacPorts users
should read [why they should switch to brew][8].



Do all this from Terminal.app; we'll swap out to iTerm2 later.
    
    $ ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"
    $ brew doctor
    $ brew install cowsay
    $ rehash
    $ cowsay "brew install worked!"

Now that brew thinks it is working, try installing a utility that was
already installed by the Command Line Tools, namely "git" (and a few
other useful utilities).

    $ brew install git wget gist htop source-highlight
    $ brew doctor



GNU utilities TCSH
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
[scottstanfield/dotfiles][9]. It uses tcsh (most
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
  downloads][10].
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
in this [github comment][11].

13. Setup Git for github
------------------------
Assuming you have a github.com account, tell your Mac about it. Follow
[these instructions][12].

    $ git config --global user.name "your name here"
    $ git config --global user.emal "your@email.com"
    $ git config --global credential.helper osxkeychain


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
This [gist][13] will help you get X11
working with Mountain Lion since it's no longer installed with OS X.


[1]: https://github.com/thoughtbot/laptop/blob/master/mac
[2]: http://zanshin.net/2013/02/02/zsh-configuration-from-the-ground-up/
[3]: https://github.com/myfreeweb/zshuery/blob/master/zshuery.sh
[4]: http://bit.ly/VQsHy1
[5]: http://www.mitchchn.me/2014/os-x-terminal/
[6]: http://drbunsen.github.io/formd/
[7]: http://brew.sh
[8]: http://lostincode.net/blog/homebrew
[9]: http://github.com/scottstanfield/dotfiles
[10]: https://code.google.com/p/iterm2/downloads/list?q=label:Featured
[11]: https://github.com/scottstanfield/newmac/issues/2
[12]: https://help.github.com/articles/set-up-git
[13]: https://gist.github.com/1860902
