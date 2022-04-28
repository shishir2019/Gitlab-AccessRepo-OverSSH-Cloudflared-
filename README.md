# Gitlab-AccessRepo-OverSSH-Cloudflared-
This article outlines steps to access Gitlab Repo over SSH/Cloudflared

The Gitlab server is only accessible via the UI (protected by Okta and Cloudflare) and SSH (protected by Cloudflare and SSH keys).  Direct HTTP(S) or SSH access is unavailable.

The Git command line tool doesn’t provide a way to log in with Okta when using HTTP(S) so only SSH is available for Git command line usage (and anything that wraps around the command line).

SSH Access is only available over cloudflared.

Install Cloudflared
See the upstream documentation here: Downloads · Cloudflare Zero Trust docs 

Log into Cloudflared
TODO

Configure SSH Access
Install an SSH client
Linux/Mac
You probably already have an SSH client installed as a part of your operating system.  If not, see your operating system documentation for instructions to install an SSH client.

Windows
Download and install cloudflared-windows-amd64.msi file from below github: 
Releases · cloudflare/cloudflared 

Download and install git for windows here: 
Git for Windows 
Alternatively you can download portable version of git here: 
Releases · sheabunge/GitPortable 

Go to your home directory in windows where you have .ssh folder or if not, you can create one. 
C:\Users\<username>\.ssh

Open git-bash.exe and then create public/private key pair as per below (Generate a public/private key pair instructions) to add ssh keys to git.orro.dev.

Add your SSH key to the ssh-agent from git-bash

$> eval "$(ssh-agent -s)"

$> ssh-add ~/.ssh/your.name@ssh-git.orro.dev


Generate a public/private key pair
The following command will generate an ed25519 ssh public/private key pair:


ssh-keygen -t ed25519 -C "your.name@ssh-git.orro.dev" -N "" -f your.name@ssh-git.orro.dev
Two files will be created in the current directory:

your.name@ssh-git.orro.dev: The private key.  This should be kept safe on your PC.  It should never be shared.  You should never need to provide this key to Gitlab.

your.name@ssh-git.orro.dev.pub: The public key.  This will be provided to Gitlab.

Add your public key to Gitlab
Configure SSH Client


Linux/Mac

Host ssh-git.orro.dev
        IdentityFile    /home/work/.ssh/identities/shaun.maher@ssh-git.orro.dev
        ProxyCommand    bash -c "cloudflared access ssh --hostname %h 2>/dev/tty"

Windows


Go to your .ssh directory in home and create a file named ‘config’  (Note: No file extension) and add below content and save it.


Host ssh-git.orro.dev
        IdentityFile    ~/.ssh/your.name@ssh-git.orro.dev
        ProxyCommand    bash -c "cloudflared access ssh --hostname %h 2>/dev/tty"
