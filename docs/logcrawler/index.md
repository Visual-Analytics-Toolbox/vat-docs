# Log Crawler
Scripts for inserting log data into the database. These scripts generally only need to be run once per game folder.

The logcrawler scripts use the [VAAPI pip package](https://pypi.org/project/vaapi/) to communicate with the [database backend](https://github.com/BerlinUnited/Visual-Analytics) and the [Naoth pip package](https://pypi.org/project/naoth/) to parse the logs. The logcrawler expects the folder structure that is described below and will only work correctly if the data is in this structure. We developed a couple of standalone scripts in the standalone folder.