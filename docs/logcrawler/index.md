# Overview
The [Berlin United Logcrawler](https://github.com/BerlinUnited/logcrawler) is a tool that sorts, cleans uploaded data recorded at RoboCup Events. This tool is tightly coupled to the way we record our log data and videos. The logcrawler runs inside our k8s infrastructure and checks every few minutes if the data is in the format described in [logstructure.md]. If the data is in the correct format it will be inserted in our postgres database which serves as basis for our analytics engine.

