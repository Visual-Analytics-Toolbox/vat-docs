# Log Structure
Our Logcrawler expects the log data to be in the format described below. There is one folder per event with the date of the first setup day. Each event folder contains a folder per halftime we play. Additionally it contains a video folder containing videos of games where we don't participate, a matches.csv file that contains information about each game of the event. Each game/halftime folder contains multiple subfolders, one for each log.


## Folder Structure Overview
```
logs/
    ...
    2026-03-10-GO26/        # folder is named after first setup day of event in yyy-mm-dd format followed by the name of the event
        comments.txt        # manually created file containing notes that are relevant for the overall event
        matches.csv         # manually created file containing the schedule and scores of all games
        2026-03-10_18-30-00_BerlinUnited_vs_Invisibles_half1-test/
            **the -test suffix indicates a testgame that we recorded similar to a real game**
        2026-03-11_11-50-00_Bit-Bots_vs_Berlin United_half1
            comments.txt
            extracted/
                1_33_Nao0022_260311-1125/
                3_35_Nao0022_260311-1125/
            game_logs/
                1_33_Nao0022_260311-1125/
                3_35_Nao0022_260311-1125/
                    **[data recorded by the robot and collected after the game from the robots]**
                    config.zip
                    game.log
                    sensor.log
                    nao.info
                    comments.txt
                ...
            videos/
                2026-03-11_11-50-00_Bit-Bots_vs_Berlin United_half1_Field-C_PiCam.mp4
                comments.txt
            gc_logs/
                **[format highly depends on the version of the game controller used]**
                **[2026 gamecontroller stores data from all halfs in one file - we just copy the file in the folders for all halfs]**
                2026-03-11_11-50-00_Bit-Bots_vs_Berlin United_half1.yaml
        2026-03-11_11-50-00_Bit-Bots_vs_Berlin United_half2
        2026-03-11_11-50-00_Bit-Bots_vs_Berlin United_half2-to
            **a timeout (to) is a normal part of the game therefore we treat it as if it would be an additional half**
            **the folder with the -to suffix contains data from when the game was resumed after timeout was called**
            **if a second timeout is called in a halftime we would use the suffix -to2**
            **penalty shootouts are handled the same way and treated as a separate half**
        videos/
            **Contains videos of other games**
            2026-03-11_11-00-00_WF Wolves_vs_ZJU Dancer_half2_Field-C_PiCam.mp4
            ...
```

The event folder, game folders, log folders, gc_logs folders and the video folder each can have a comments.txt file 

## matches.csv Format
```csv
date,time,team1,team2,score,referees,field,division
2026-03-11,10:30,Berlin United,HULKs,0:0,[REF R-ZWEI KICKERS & Ruhrbot Devils],Field-B,M
```
