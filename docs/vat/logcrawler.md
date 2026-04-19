
The [Berlin United Logcrawler](https://github.com/Visual-Analytics-Toolbox/logcrawler) is a tool that sorts, cleans uploaded data recorded at RoboCup Events. This tool is tightly coupled to the way we record and store our log data and videos. The logcrawler runs inside our k8s infrastructure and checks every few minutes if the data is in the format described in [logstructure.md]. If the data is in the correct format the log data is indexed and the index is written to a postgres database which serves as basis for our analytics tools.

All images and videos are also added to our labelstudio instance and our current best models will be used to automatically annotate the image data. The automatic labeling happens inside a different cron job.

# Setup
Basic setup is explained in the repos README.md: [https://github.com/Visual-Analytics-Toolbox/logcrawler](https://github.com/Visual-Analytics-Toolbox/logcrawler)


For development of the logcrawler functions it might be useful to access the filesystem directly from your development machine. If you are a member of the Berlin United group you can mount the the filesystem via sshfs directly. Then set the `VAT_LOG_ROOT` variable accordingly.
```bash
sudo sshfs <user>@<gruenau server>:/vol/repl261-vol4/naoth/logs/ /mnt/repl -o allow_other,uid=33,gid=33,ServerAliveInterval=15,ServerAliveCountMax=3,reconnect -o IdentityFile=~/.ssh/id_ed25519
```