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
> \> Set-ExecutionPolicy RemoteSigned -scope CurrentUser
> \> iwr -useb get.scoop.sh | iex

### Install Cmder

Next, we'll use Scoop to install our terminal replacement, Cmder.
To do so, run the following in **Powershell**:
> scoop install cmder

## Configuring Cmder

Go ahead and open up Cmder (you can do so by searching for it after hitting the **Windows key**).
Navigate to the settings (**Win**+**Alt**+**P**).
