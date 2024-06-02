---
layout: post
title:  "Downgrading Firefox"
---

Firefox recently started spamming me with update box dialogues. As it happened that unfortunate day, I misclicked on "Update" instead of closing the dialogue box. Everything broke for me. Videos, full screen, etc. I wanted to roll back but downloading older releases and booting them resulted in infinite loops.

These instructions are for OSX:

- Install your version of choice from the Firefox [FTP](https://ftp.mozilla.org/pub/firefox/releases/) server.
- Back up whatever you don't want to lose.
    - See this for backing up [bookmarks](https://support.mozilla.org/en-US/kb/export-firefox-bookmarks-to-backup-or-transfer).
- `$cd ~/Library/Application Support/Firefox/`
- `$rm -rf *`
- Open the new Firefox to see if it works. Then, close it.

I also wanted to completely disable checking for updates.
- Quit Firefox.
- `$sudo defaults write /Library/Preferences/org.mozilla.firefox EnterprisePoliciesEnabled -bool TRUE`
- `$sudo defaults write /Library/Preferences/org.mozilla.firefox DisableAppUpdate -bool TRUE`
- Boot up Firefox and go to `about:policies`. You should see `DisableAppUpdate` set to True.
- You can import your bookmarks and set preferences in `about:preferences`. The Preferences panel will say "Your browser is being managed by your organization". This is because, in order to set policies, you need to enable Enterprise Policies. See [this](https://github.com/mozilla/policy-templates/tree/master/mac). Anyhow, you needn't be worried about this statement. It won't affect your browser experience whatsoever.