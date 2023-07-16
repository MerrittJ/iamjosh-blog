+++ 
date = 2023-07-16T20:22:38+01:00
title = "Setting up a development environment on the Steam Deck"
description = ""
slug = "setting-up-development-environment-on-Steam-Deck"
authors = []
tags = ['steam-deck']
categories = []
externalLink = ""
series = []
+++

# Introduction

This is a short tutorial on how to quickly get a Steam Deck ready for developing, with some gentle bias towards setting things up for developing websites or JavaScript apps (as that's my background). 

The Steam Deck filesystem is almost entirely read-only. Unlocking it to be writable isn't the fix because of the A/B partitions used by SteamOS; writes to a read-only area will not persist between OS updates. Fortunately, we can get around this by working entirely within the writable directories `/home` and `/etc`. How do we ensure everything we install is saved to these locations? Distrobox!

I stand on the shoulders of giants, namely Ian Wootten, who has excellent videos and articles on getting the Steam Deck ready to develop, and Luca di Maio with the Distrobox tool and guides. I want to combine and add a few extra steps to these tutorials in case anyone (like me) got stuck.

In this tutorial, we will:
- [Install Distrobox](#distrobox)
- [Install VSCode](#vscode)
- [(Optional) Install some extra nice-to-have tools for developing](#extras)

# Distrobox

Distrobox is an application that lets us run Linux distributions in containers, accessible by command line. This is useful because we can install Distrobox to `/home` then when we run a Linux distro, what the distro thinks is its `/bin`/`/lib`/et cetera is actually all within `/home`. This fixes the main issue of any installer wanting to write to a read-only part of the SteamOS filesystem.
Distrobox uses Podman or Docker under the hood but I opted for Podman. They're pretty interchangeable but I've had fewer issues with Podman than Docker in my career.

So, let's get into it. opening up Konsole (terminal for the KDE desktop)

1. Open up Konsole (terminal for the KDE desktop)
2. Set a password for the default user with `passwrd`. This will let us use `sudo` later.
3. Run the following commands
    ```bash
    curl -s https://raw.githubusercontent.com/89luca89/distrobox/main/install | sh -s -- --prefix ~/.local
	curl -s https://raw.githubusercontent.com/89luca89/distrobox/main/extras/install-podman | sh -s -- --prefix ~/.local
    ```
    
    This installs Distrobox and Podman into the `/home/.local` directory
4. Add the bins to your PATH (in your .bashrc/.zshrc or equivalent)
    ```bash
    export PATH=$PATH:$HOME/.local/bin
    export PATH=$PATH:$HOME/.local/podman/bin
    ```
5. Add the Podman bin to the ~/.distroboxrc file (create one if it doesn't exist yet)
	```bash
    export PATH=$PATH:$HOME/.local/podman/bin
    ```
   
   This lets us run Distrobox with Podman from the terminal now.
6. Also add an xhost command to **both** the .bashrc or equivalent file **and** .distroboxrc file
    ```bash
    xhost +SI:localuser:$USER
    ```
    
    This lets us run graphical applications from Distrobox
7. Create your desired Linux container. I used Ubuntu 22:10
	```bash
    distrobox create -i ubuntu:22.10
    ```
	
    At this stage, it's worth verifying the create worked by entering the container
	```bash
    distrobox enter ubuntu-22-10
    ```

Now we have Distrobox installed and we can use it with an Ubuntu distro. Inside this distro we can install anything we want and it will persist between SteamOS updates.

# VSCode

Everyone has their own preferences for IDE but I'm enjoying VSCode. The following steps will probably be relevant to your preferred IDE.

1. Download the latest VSCode .deb file from Microsoft
2. While inside your new container and in the home directory, install VSCode 
	```bash
    sudo apt install ./Downloads/code_1.76.2-1678817801_amd64.deb
    ```
3. Export this newly installed app to your host with
	```bash
    distrobox-export --app code
    ```
	After a moment, the new VSCode app should be available to see in your main list of programs/apps. Open it, verifying the title says something like "Visual Studio Code (on Ubuntu 22:10)"


# Extras

This section is for other tools I installed after setting up the above. These are truly preferences and specific to how and what I develop.

## NVM
Installs by `curl`
1. Download
    ```bash
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
    ```

2. The above adds NVM to your PATH automatically so open a new terminal and run 
    ```bash
    nvm install node
    ```

## Git
Installs by `apt-get`. Remember we're working in a non-SteamOS distro now.
```bash
sudo apt-get install git
```

## Homebrew
Requires Git, installs by `curl` 
1. Download
    ```bash
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    ```
2. Run the commands it prints at the end. These depend on your distro but if you use an Ubuntu container like me, run the following
    ```bash
    sudo apt update && sudo apt install make build-essential
    ```

## Terraform
Requires Homebrew, installs by Homebrew
```bash
brew tap hashicorp/tap
brew install hashicorp/tap/terraform
```


# References:
- Distrobox - https://www.ianwootten.co.uk/2022/11/30/how-to-setup-distrobox-on-the-steam-deck/ (up to the Pyenv section)
- VSCode: https://code.visualstudio.com/
- VSCode + Distrobox - https://github.com/89luca89/distrobox/blob/main/docs/posts/integrate_vscode_distrobox.md
- NVM - https://github.com/nvm-sh/nvm#installing-and-updating

