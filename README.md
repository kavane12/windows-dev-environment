# Setting up a Windows Dev Environment

This step-by-step guide aims to bring terminal-like functions normally found in Linux / Unix to Windows. Functionalities include:

**Cmder**: A powerful console editor to replace Powershell
**Oh-my-posh**: Oh-my-zsh on windows!
**posh-git**: Use git from the command-line
**autojump**: Easily navigate to a directory from anywhere

## Installing Scoop

Scoop is a command-line installer for Windows, comparable to Homebrew for macOS. We'll use it to install most of our environment.

### Installation

To install Scoop, open up **Powershell** and run the following:
```
> Set-ExecutionPolicy RemoteSigned -scope CurrentUser
> iwr -useb get.scoop.sh | iex
```

### Install Cmder

Next, we'll use Scoop to install our terminal replacement, Cmder.
To do so, run the following in **Powershell**:
```
> scoop install cmder
```

## Configuring Cmder

Cmder is great but not perfect right out of the box. We'll make some changes to modernize our experience with it.

### Settings

Go ahead and open up Cmder (you can do so by searching for it after hitting the **Windows key**).
Navigate to the settings (**Win+Alt+P**).

Change the following settings:

- Under **Fonts**, check the box for **Treat font height as device units**. Feel feel to change the font.

- Under **Size & Pos**, change **Window size** to **Normal**. Select and width and height that feels appropriate for your screen.
- Under **Quake style**, check the boxes for **Quake style slide down**, **Auto-hide on focus lose**, and **Always on top**. Optionally change **Animation time (ms)** to 0. Allows Cmder to exist as a program overlay instead of a window. Use **Ctrl+`** to open / close Cmder from anywhere.
- Under **Tab bar**, uncheck the box for **Tabs on bottom**. This should move the tabs to the top of the window.
- Under **Confirm**, uncheck **Confirm creating new console/tab**. 
	- Then navigate to **Keys & Macro** and find the hotkey for **Ctrl+T** (which should be bound to *Create new console (Create confirmation dialogue)*)
	- Find *Create new console or new window (check 'Multiple consoles in one ConEmu window'): Create()* and set its hotkey to **Ctrl+T**.
- Under **Startup**, select the dropdown for **Specified named task** and choose **{Powershell::Powershell}**. This makes Powershell the default program to run on startup or new tab.

There's still some changes to made to Cmder, but we'll need other packages first.

### Oh-my-posh & posh-git

[posh-git](https://github.com/dahlbyk/posh-git)
[oh-my-posh](https://github.com/JanDeDobbeleer/oh-my-posh)

Oh-my-posh and posh-git will change the appearance of Cmder's powershell to be more readable and useful. Install by running the following:
```
> Install-Module posh-git -Scope CurrentUser
> Install-Module oh-my-posh -Scope CurrentUser
```
Then we'll create a profile file for Cmder to call on startup (so it starts with posh-git and oh-my-posh):
```
> if (!(Test-Path -Path $PROFILE )) { New-Item -Type File -Path $PROFILE -Force }
```
Open the file by running:
```
notepad $PROFILE
```
and copy the following into the opened file (don't forget to save):
```
# Aliases
function getGitStatus { & git status $args }
New-Alias -Name gs -Value getGitStatus -Force -Option AllScope
function getGitCommit { & git commit -m $args }
New-Alias -Name gcm -Value getGitCommit -Force -Option AllScope
function getGitAdd { & git add $args }
New-Alias -Name ga -Value getGitAdd -Force -Option AllScope
function getGitPush { & git push $args }
New-Alias -Name gps -Value getGitPush -Force -Option AllScope
function getGitPull { & git pull $args }
New-Alias -Name gpl -Value getGitPull -Force -Option AllScope
function getGitFetch { & git fetch $args }
New-Alias -Name gf -Value getGitFetch -Force -Option AllScope
function getGitCheckout { & git checkout $args }
New-Alias -Name gco -Value getGitCheckout -Force -Option AllScope
function getGitBranch { & git branch $args }
New-Alias -Name gb -Value getGitBranch -Force -Option AllScope
function getGitRemote { & git remote $args }
New-Alias -Name gr -Value getGitRemote -Force -Option AllScope
function getGitLog { & git log }
New-Alias -Name gl -Value getGitLog -Force -Option AllScope
function getGitDiff { & git diff }
New-Alias -Name gd -Value getGitDiff -Force -Option AllScope

# Ensure posh-git is loaded
Import-Module posh-git

# Start SshAgent if not already
# Need this if you are using github as your remote git repository
# if (! (ps | ? { $_.Name -eq 'ssh-agent'})) {
#     Start-SshAgent
# }

# Ensure oh-my-posh is loaded
Import-Module oh-my-posh

# Default the prompt to agnoster oh-my-posh theme
Set-Theme agnoster

# Set default user (CHANGE THIS TO YOUR OWN USERS NAME)
$DefaultUser = ''

# Ensure autojump is loaded
Import-Module Jump.Location
```

Make sure to change **$DefaultUser** to your own username. This is your Windows account name. If your not sure, navigate to **C:/Users** and select a username from among the folders.

Now to get Cmder to use the new file, once again open up Cmder's settings, navigate to **Tasks**, and select **{PowerShell::PowerShell}**. Replace *PowerShell -ExecutionPolicy Bypass -NoLogo -NoProfile -NoExit -Command "Invoke-Expression 'Import-Module ''%ConEmuDir%\..\profile.ps1'''"* with: 
```
PowerShell -ExecutionPolicy Bypass -NoLogo -NoProfile -NoExit -Command "Invoke-Expression '. "%USERPROFILE%\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1"'" -new_console:d:"%USERPROFILE%"
```

### Fonts
[nerd-fonts](https://github.com/ryanoasis/nerd-fonts)

The default fonts are missing some support for various symbols used by oh-my-posh / posh-git, so a customized one is best.
Install a font (or multiple) from the link above.

From the link above, navigate to **Patched fonts**, select a desired font, then size. Select **Regular > Complete** and then pick the file that has **Mono** and **Windows compatible** in its name. Download the file.

My personal choice: [Meslo LG M Regular](https://github.com/ryanoasis/nerd-fonts/blob/master/patched-fonts/Meslo/M/Regular/complete/Meslo%20LG%20M%20Regular%20Nerd%20Font%20Complete%20Mono%20Windows%20Compatible.ttf)

After downloading, right-click the file and select **Install**. Windows should install the font.

Open up Cmder's settings, and change the font in three places:

- **General > Fonts > Main console font**
- **General > Fonts > Alternative font**
- **Tab bar > Tab font**

### Colorized output
[Get-ChildItemColor](https://github.com/joonro/Get-ChildItemColor)

To get colorized output in Cmder, we'll use Get-ChildItemColor. Run the following:
```
> Install-Module -AllowClobber Get-ChildItemColor -Scope CurrentUser
```
Then, add the following to your profile (open with `notepad $PROFILE`):
```
# Ensure that Get-ChildItemColor is loaded
Import-Module Get-ChildItemColor

# Set l and ls alias to use the new Get-ChildItemColor cmdlets
Set-Alias l Get-ChildItemColor -Option AllScope
Set-Alias ls Get-ChildItemColorFormatWide -Option AllScope
```

### Jump
[Jump-Location](https://github.com/tkellogg/Jump-Location)

Jump-Location allows the user to jump to any previously visited directory from anywhere. To install, run the following:
```
> Install-Module Jump.Location -Scope CurrentUser
```
Then, add the following to your profile (open with `notepad $PROFILE`):
```
# Ensure autojump is loaded
Import-Module Jump.Location
```

### Others
Miscellaneous quality of life changes.

- Shorten *C:/User/Username* to just *~* by adding the following to your profile.
```
# Helper function to change directory to my development workspace
# Change c:\ws to your usual workspace and everytime you type
# in cws from PowerShell it will take you directly there.
function cws { Set-Location C:\Users\[YOUR USERNAME]\Desktop }
```


## Installing dev packages

We'll install many of the most used and helpful packages through scoop. Run the following:
```
> scoop install git
> scoop bucket add extras
> scoop bucket add versions

> scoop install python

> scoop install nodejs
> scoop install nvm

> scoop install vscode
> scoop install vim
```

## Configuring Visual Studio Code
VS code has a ton of plugins to choose from, all of which make coding easier and more   productive. Below are the ones I use:
```
Atom One Dark Theme
Bracket Pair Colorizer
DotENV
ESLint
GitLens
Javascript (ES6) code snippets
LintLens
markdownlint
npm
npm intellisense
Path intellisense
PowerShell
Prettify JSON
Python
Reactjs code snippets
SCSS Formatter
SCSS Intellisense
Todo Tree
Trailing Spaces
vscode-icons
vscode-position
```
To install new plugins, open up vs code (`code .` from command line) and on the left hand side, select the **extensions** icon. Search for any plugin and hit install, that's it!

## Now you're all set!
Your dev environment on your Windows device is now done. Time to code!
