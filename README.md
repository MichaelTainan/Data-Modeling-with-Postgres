# Project1 Data Modeling with Postgres

## 1. Goal in the Project
Sparkify's app just used the JSON format to log their user's behavior when they used the app in period.
Because JSON format metadata is not easy to analysis, theay want to find a team or person(like us) to help them.
Help them to analyze their users listen what songs or what time to listen and so on. So for this purpose, we had to design a star schema data model to transfer these JSON datas into these relational DB's table to help them reach their goal.

## 2. State and justify my design table schema and ETL
### 2.1. design the Tables' Schema 
#### 2.1.1. Users Dimension table
Store the app's user infomation, source is came from log_data, ETL input 96 rows
#### 2.1.2. Songs Dimension table
Store the song's infomation, like song's title, duration, artist_id, source is came from song_data, ETL input 71 rows
#### 2.1.3. artists Dimension table
Store these song's artist infomation, like atrist's name, and their location and loation's coordinate , source is came from song_data, ETL input 69 rows
#### 2.1.4. time Dimension table
Store these user what time they start used the app to listen these songs, and recognize the start time and separate it, purpose for analyze , source is came from log_data, ETL input 6813 rows
#### 2.1.5. songplays Fact table
Store these logdata have to match title in songs table and artist's name in artists table and song's duration in songs table, then this fact table can privide Sparkfy to use these Dimension tables to analyze the log data, source is came from log_data, ETL input 6820 rows
### 2.1.6 ETL
Used the python to create process_data function to extract the log_data and song_data files, then create process_song_file to load the data to used for songs table and artists table, we just fetch first row in every  file, if it's log_data, we create process_log_file function to load it to users table and transfer ts attribue data become time table, use pandas to_datetime function transfer ts data to timestamp, unit='ms', then fetch it's hour, day, week, month and weekday, then recognize these data to become the time dimesion table. When we want create songplays fact table's data, we check the log data if it can't equal song table's song title, duration and artist table's name, we set song_id, artist_id equal =None. then load it into songplays table. 
### 2.1.7 Execute project file step
1. run Run_py_file.ipynb I create: it would be run create_tables.py and etl.py code.
2. run test.ipynb: to check the result if is currect. I had added count SQL to count every table's rows.

## 3. Provide example queries and results for song play analysis.
### 3.1. Used songs and artists dimension table to find song plays title and artist name 
%sql SELECT songs.title, artists.name, songplays.start_time FROM (songplays JOIN songs 
ON songplays.song_id = songs.song_id) JOIN artists on songplays.artist_id = artists.artist_id

result: 1 row
        title	name	start_time
Setanta matins	Elena	1542837407796

### 3.2. Used users dimension table to count every user play songs number of times
%sql SELECT users.first_name || ' ' || users.last_name as username, count(*) FROM songplays JOIN users 
ON songplays.user_id = users.user_id GROUP BY users.user_id

result: 96 rows
username	count
Jahiem Miles	11
Kaylee Summers	27
Christian Porter	2
Tegan Levine	665
Rylan George	223
Walter Frye	2
Kaleb Cook	27
Kimber Norris	3
Isaac Valdez	3
Samuel Gonzalez	24
Cierra Finley	13
Colm Santana	25
Dustin Lee	1
Theodore Harris	22
Ann Banks	4
Marina Sutton	3
Brantley West	7
Stefany White	27
Makinley Jones	7
Matthew Jones	248
Noah Chavez	7
Lily Koch	463
Kevin Arellano	37
Magdalene Herman	10
Alivia Terrell	5
Kynnedi Sanchez	10
Avery Watkins	178
Ava Robinson	48
Jacob Klein	289
Bronson Harris	20
Sara Johnson	213
Tucker Garrison	7
Cienna Freeman	2
Celeste Williams	20
Connar Moreno	10
Zachary Thomas	9
Katherine Gay	8
Ryann Smith	27
Maia Burke	15
Morris Gilmore	4
Molly Taylor	16
Amiya Davidson	17
Theodore Smith	17
Jayden Fox	55
Jayden Duffy	14
Anabelle Simpson	29
Aiden Ramirez	9
Aleena Kirby	397
Jordan Hicks	34
Kinsley Young	179
Avery Martinez	87
Evelin Ayala	9
Sienna Colon	6
Lily Burns	56
Jayden Graves	169
Austin Rosales	12
Emily Benson	140
Sylvie Cruz	28
James Martin	2
Ryan Smith	114
Harper Barrett	140
Andrea Butler	3
Jacob Rogers	5
Chloe Roth	14
Lily Cooper	2
Jordyn Powell	4
Adler Barrera	19
Aiden Hess	45
Chloe Cuevas	689
Ava Robinson	5
Sean Wilson	2
Ayla Johnson	20
Wyatt Scott	16
Layla Griffin	321
Jayden Bell	9
Jaleah Hayes	33
Hannah Calhoun	2
Dominick Norris	1
Martin Johnson	15
Carlos Carter	2
Jordan Rodriguez	3
Shakira Hunt	6
Mohammad Rodriguez	270
Gianna Jones	3
Devin Larson	18
Braden Parker	8
Cecilia Owens	23
Ayleen Wise	5
Jacqueline Lynch	346
Jizelle Benjamin	10
Hayden Brock	72
Kate Harrell	557
Brayden Clark	9
Joseph Gutierrez	18
Elijah Davis	4
Adelyn Jordan	5




