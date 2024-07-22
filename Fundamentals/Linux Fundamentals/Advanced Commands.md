```sh
# Complete Update
sudo apt update -y && sudo apt full-upgrade -y && sudo apt autoremove -y && sudo apt autoclean -y

# Customize Bash prompt
┌─[cry0l1t3@parrotos]─[~]
└──╼ $ cp .bashrc .bashrc.bak
┌─[cry0l1t3@parrotos]─[~]
└──╼ $ echo 'export PS1="-[\[$(tput sgr0)\]\[\033[38;5;10m\]\d\[$(tput sgr0)\]-\[$(tput sgr0)\]\[\033[38;5;10m\]\t\[$(tput sgr0)\]]-[\[$(tput sgr0)\]\[\033[38;5;214m\]\u\[$(tput sgr0)\]@\[$(tput sgr0)\]\[\033[38;5;196m\]\h\[$(tput sgr0)\]]-\n-[\[$(tput sgr0)\]\[\033[38;5;33m\]\w\[$(tput sgr0)\]]\\$ \[$(tput sgr0)\]"' >> .bashrc

-[Tue Mar 23-00:39:51]-[cry0l1t3@parrotos]-
-[~]$ 


# Search for apt packages locally
axi-cache search forensics graphical

# Setup nuke password to destroy all data (Enter this instead of actual password)
dpkg-reconfigure cryptsetup-nuke-password

# Run multiple commands in one-line with output redirection
ls | xargs -I word -n 1 -t sh -c 'echo word >> shortrockyou; rm word'
```

