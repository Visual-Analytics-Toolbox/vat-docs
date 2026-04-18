
The [Berlin United Logcrawler](https://github.com/Visual-Analytics-Toolbox/logcrawler) is a tool that sorts, cleans uploaded data recorded at RoboCup Events. This tool is tightly coupled to the way we record and store our log data and videos. The logcrawler runs inside our k8s infrastructure and checks every few minutes if the data is in the format described in [logstructure.md]. If the data is in the correct format the log data is indexed and the index is written to a postgres database which serves as basis for our analytics tools.

All images and videos are also added to our labelstudio instance and our current best models will be used to automatically annotate the image data. The automatic labeling happens inside a different cron job.

# Log Crawler Steps
TODO