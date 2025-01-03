# Full Mac Setup Process (for Ryan McCawley)

There are some things in life that just can't be automated... or aren't 100% worth the time :(
This document covers that, at least in terms of setting up a brand new Mac out of the box.

## Initial configuration of a brand new Mac

Before starting, I completed Apple's mandatory macOS setup wizard (creating a local user account, and optionally signing into my iCloud account). Once on the macOS desktop, I do the following (in order):


- Install Ansible (following the guide in [README.md](README.md))
- **Sign in in App Store** (since `mas` can't sign in automatically)
- Clone mac-dev-playbook to the Mac: `git clone git@github.com:mccawleyr/mac-dev-playbook.git`
- Drop `config.yml` from `~/Dropbox/Apps/Config` to the playbook (copy over the network or using a USB flash drive).
- Run the playbook with `--skip-tags post`.
  - If there are errors, you may need to finish up other tasks like installing 'old-fashioned' apps first (since I try to place Photoshop in the Dock and it can't be installed automatically). Then, run the playbook again.
- Start Synchronization tasks:
  - Open Photos and make sure iCloud sync options are correct
  - Open Music, make sure computer is authorized, and set Library sync options
- Manually copy `~/Development` folder from another Mac (to save time).
- Manual settings to automate someday:

- System Settings:
  - Extensions
  - Login Items and Extensions - Sharing
- Siri
  - Listen for "Siri or Hey Siri"
  - Command key twice to trigger siri

- Finder Settings
  - Shows these items on the desktop - everything
  - New finder window shows - Desktop
  - Sidebar adjustments to preferred folders
  - Advanced / show all file name extenions
  - View / Customize Toolbar / Add Airdrop to toolbar
  - Show path bar
  - Show status bar
  - View options
  



  - _After Dropbox Sync completes_: Run the playbook with `--tags post` to complete setup.
  - Symlink the synchronized `config.yml` into the playbook dir: `ln -s /Users/jgeerling/Dropbox/Apps/Config/mac-dev-playbook/config.yml /Users/jgeerling/Development/mac-dev-playbook/config.yml`
  - These things might be automatable, but I do them manually right now:
    - Configure Time Machine backup drive and set Time Machine backups to daily instead of hourly
    - Install Wireguard from App Store and add configuration (if needed)

## To Wrap in Post-provision automation

The following tasks have to wait for the initial Dropbox sync to complete before they'll succeed. So ideally I'll stick this all in a post-provision script but somehow flag it not to run on first provision.

```
# SSH setup.
ssh-keygen  # and create a default key to set up .ssh folder
# TODO - Manually copy any shared SSH keys that are needed.


# Vim setup.
mkdir -p ~/.vim/autoload
mkdir -p ~/.vim/bundle
cd ~/.vim/autoload
curl https://raw.githubusercontent.com/tpope/vim-pathogen/master/autoload/pathogen.vim > pathogen.vim
cd ~/.vim/bundle
git clone git://github.com/scrooloose/nerdtree.git
```

## When formatting old Mac

  - Sign out of Adobe Creative Cloud
  - Sign out of Panic Sync in Transmit
  - Deauthorize Apple Music in iTunes/Music App
  - Make sure anything new merged into `~/Dropbox/Apps/Config`:
    - Fonts from ~/Library/Fonts
    - Motion Plugins from ~/Movies/Motion
    - Final Cut Pro Text Styles in ~/Library/Application Support/Motion/Library/Text Styles
    - Sequel Ace shortcuts from ~/Library/Containers/com.sequel-ace.sequel-ace/Data/Library/Application\ Support/Sequel\ Ace/Data/Favorites.plist
  - Follow Apple's guide [here](https://support.apple.com/en-au/HT212749)
