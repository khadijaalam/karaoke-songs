# What makes a good karaoke song?

## Content

<br>

## The Project
This project stemmed from a question I had: What makes a good karaoke song? I wanted to figure out what quantitative information I could use to find any commonalities or trends between the songs that people like singing at karaoke. <br>

<br>

## Findings
* **Pop is the most prevalent genre.** More songs were pop songs than any other genre combined.
* **People like older songs.** The 2000s were the decade with the most popular karaoke songs, followed by the 1990s. There are no songs from the 2020s (the most recent song in the dataset is from 2018).
* **A song doesn't have to be a hit on the charts in order to be a popular karaoke song.** Some of the songs were hits on the charts (and some made it onto more than one year-end hot 100 chart!) but others didn't.
* **A song doesn't necessarily require a wide vocal range in order to be a popular karaoke song**, but the two most popular karaoke songs in my dataset required the smallest vocal range.

<br>

## Data Wrangling
**[Spotify API](https://developer.spotify.com/documentation/web-api) via [spotipy](https://spotipy.readthedocs.io/en/2.26.0/)** <br>
I chose three karaoke playlists on Spotify with a large number of followers: Best Karaoke Songs Ever by Teen Vogue (147k followers), Karaoke Classics by Spotify (386k followers), and Karaoke Hits by Spotify (243k followers). Because Spotify doesn't let you access playlists it creates via the API, I created copies of those playlists with my own account so that I could access the songs. <br>

From there, I focused on the songs that appeared at least twice across the three playlists as a proxy for popularity. I ended up with 43 songs. <br>

The release dates I got were formatted inconsistently. For most songs, it was in a YYYY-MM-DD format, and for others I only got the year. Also, Spotify provided the release date of the album a song appeared on, not the song itself. This might not be 100% accurate because a song could have been released as a pre-release single. <br>

I knew that I wanted to group the songs by the decade they were released in for my dataviz so I used the to_datetime function in pandas and then did some math to get the release year for each song and convert it into the decade it was released in. <br>

**[Last.fm API](https://www.last.fm/api)** <br>
The Spotify API didn't provide the genre for each song. After testing the Last.fm API's "top tags" endpoint, I saw that the the top tag for some of the songs was genre. However, top tags are created by users, so what this endpoint really tells you is what genre most users classified a song as. <br>

At this point, I had to split the "artists" column in my Spotify dataframe into two so that I could search Last.fm by the song name and the main artist's name. <br>

But when I got the results back from Last.fm, I realized that top tags wasn't the most accurate proxy for genre. For example, the second most popular "genre" it showed me was "80s." <br>

So I had to clean up my genre column. Some songs fell under multiple genres, so I looked at the genre that was listed first on the song's Wikipedia page and classified it under that. This meant reassigning the five songs tagged "80s" and the one song tagged "motown" (I Want You Back by the Jackson 5) as "pop". I also grouped together "rock" and "hard rock." <br>

**[GetSongBPM API](https://getsongbpm.com/api)** <br>
As part of my initial analysis, I wanted to look at the BPM of each of my popular karaoke songs. Neither Last.fm not Spotify provided data on tempo, so I found a website called GetSongBPM.com that has an API. <br>

However, after querying the data and doing a first pass on it, I was unsure about its accuracy. For example, the BPM for Poker Face by Lady Gaga was listed as 234 which doesn't seem right. <br>

Given this and time constraints, I chose not to include any BPM analysis in the story. <br>

**[billboard.py API](https://github.com/guoguo12/billboard-charts)** <br>
I used billboard.py to get the Hot 100 Year-End charts from 1975 (when the oldest song in my dataset was released) to 2025 and put all of them into a dataframe. I used the yearly version of the chart instead of the weekly version so that I'd be working with a more manageable dataset. <br>

I then used a for loop to iterate through the Billboard charts to find how many times my karaoke songs appeared and their best rank on the chart. <br>

But some of the songs in my karaoke dataset were remasters, so it's possible that, for example, the original version of the song charted. <br>

**[Singing Carrots](https://singingcarrots.com/search) and [Inspired Acoustics](https://inspiredacoustics.com/en/MIDI_note_numbers_and_center_frequencies)** <br>
I wanted to figure out if a song was difficult to sing, and I decided to focus on finding the lowest and highest notes in the vocal line to see the range required to sing the song. <br>

I found a toolkit for doing musical analysis called [music21](https://music21.org/music21docs/). I planned to give it sheet music from MuseScore to get the lowest and highest notes in the vocal line. But I didn't realize that I had to pay MuseScore in order to download the sheet music as MusicXML files. <br>

I started manually looking at the sheet music to hand-collect this data but since sheet music on MuseScore is uploaded by users, I realized that some of them weren't totally accurate. <br>

I found a platform called Singing Carrots that had information on the vocal range required for songs, so in the interest of time, I collected that data for the nine songs that appeared in all popular three karaoke playlists. <br>

But the vocal ranges on Singing Carrots were written in scientific pitch notation, which would be a problem when trying to create dataviz. So I wanted to convert those notes into their MIDI note number so that I ended up with raw numbers to represent the pitches. I found a page on the website for a company called Inspired Acoustics that had a handy chart for comparing different musical notation standards and manually did the conversions. <br>

<br>

## Skills and Growth
I wanted to use this project to practice and get better at:
* **Working with APIs.** Previously, whenever I came across an API I wanted to try using, the documentation overwhelmed and scared me. So as exposure therapy, I used four APIs for this project! 
* **For loops**. Writing these is something I've been struggling with, but they're really useful. I had to use a lot of these for this project and I feel slightly more comfortable with them, which is a win for me.

I also learned some cool new tricks, like:
* Concatenating and merging dataframes.
* Creating tuples.
* Using the pandas to_datetime function.

Something I really struggled with was the fact that my skills and my ambition are not at the same level. This project is a very scaled down of what I wanted it to be, and it took some time for me to come around to that, but I eventually did! 

<br>

## For Next Time
If I had more time and abilities, I would:
* **Expand the scope of my dataset.** Ideally, I'd have liked to request data from karaoke places/machines about which songs people play the most. Or look at more songs from Spotify so that I'd have a larger dataset, including songs from more user-generated playlists. I'd also look at the Hot 100 weekly charts instead of the year-end ones for a more comprehensive view of commercial popularity.
* **Do more data cleaning.** I realized too late that my Billboard chart analysis might not tell the full picture since my dataset included remastered versions of songs and it could be the case that the original version of the song charted.
* **Find another data source for genres.** Maybe scraping Wikipedia would've been more accurate than using the Last.fm API.
* **Potentially include other points of analysis.** I could've had like a million charts in the story, but it's probably good that I didn't. Still, I ended up with some nuggets of analysis that I could flesh out.
    - The average length of the songs is almost four minutes long.
    - Some artists like Britney Spears have two songs in the dataset.
    - There aren't that many duets — only eight out of the 43 songs.
* **Create more interesting dataviz**. I ended up with a lot of bar charts (save for a range plot of vocal ranges (meta!) towards the end). If I hadn't spent most of my time on data wrangling, I could've spent more time thinking about other ways to present the data. I'd also wanted to try creating dataviz with something other than Datawrapper, but maybe that's something to focus on for a future project!
* **Make it interactive.** I had a fun idea to prompt the user to sing a line of a karaoke song and grade them on it like a karaoke machine as a way of conveying how difficult a song is to sing. My friend Darryl Laiu did a similar thing for a story on [influencer uptalk](https://dlaiu.github.io/tiktok-accent/) and talked me through how I could steal his code to implement my idea, so I do hope to update my project to include this! 
