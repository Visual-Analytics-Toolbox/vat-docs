# LogCrawler Development
For development of the logcrawler functions it might be useful to access the filesystem directly from your development machine. If you are a member of the Berlin United group you can mount the the filesystem via sshfs directly. Then set the `VAT_LOG_ROOT` variable accordingly.

```bash
sudo sshfs naoth@gruenau4.informatik.hu-berlin.de:/vol/repl261-vol4/naoth/logs/ /mnt/repl -o allow_other,uid=33,gid=33,ServerAliveInterval=15,ServerAliveCountMax=3,reconnect -o IdentityFile=~/.ssh/id_ed25519
```