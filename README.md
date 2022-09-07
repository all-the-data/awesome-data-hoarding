# awesome-data-hoarding

A single-page cheat sheet of commands and tools for scraping, saving, hoarding, archiving, collecting, organising and browsing data.

Inspired by Reddit's [/r/DataHoarder](https://www.reddit.com/r/DataHoarder/)

## Types of content

Which archiving tool should you choose?

- Website: wget or [ArchiveBot](https://wiki.archiveteam.org/index.php?title=ArchiveBot)

- Reddit thread: TO DO

- Discord: DiscordChatExporter

- Youtube video/music: youtube-dl

## Scraping tools

### General purpose

- [wget](https://www.gnu.org/software/wget/manual/wget.html)

```
wget \
    -e 'robots=off' \
    --accept '*.*' \
    --mirror \
    --wait 2 \
    --random-wait \
    --convert-links \
    --user-agent 'Mozilla/5.0 (Windows NT 10.0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.7113.93 Safari/537.36' \
    'http://www.example.com/'
```

- [StreamRipper](http://streamripper.sourceforge.net)
    - Example: `streamripper ###URL### -u "FreeAmp/2.x" -q -l 86400`

- [Chrome DevTools](https://developer.chrome.com/docs/devtools)
    - [network tab](https://developer.chrome.com/docs/devtools/network/reference)
    - [resources tab](https://developer.chrome.com/docs/devtools/resources)

### Specialised

- [youtube-dl](https://yt-dl.org)
    - Example: `youtube-dl ###URL### --ignore-errors --extract-audio --audio-quality 0 --audio-format mp3 --prefer-ffmpeg --output "%(title)s.%(ext)s" `
    - Playlist: `youtube-dl ###URL### ...etc... --output "%(playlist_title)s/%(playlist_title)s - %(playlist_index)02d - %(artist)s - %(title)s.%(ext)s"`
    - Album: `youtube-dl ###URL###  ...etc... --output "%(album)s - %(artist)s - %(track)s.%(ext)s"`

- [DiscordChatExporter](https://github.com/Tyrrrz/DiscordChatExporter) + excellent [wiki](https://github.com/Tyrrrz/DiscordChatExporter/wiki)
    - Example: `docker run --rm -v /var/www/zaphod/adhd:/app/out tyrrrz/discordchatexporter:stable export --channel ###ID### --token ###SECRET### --format Json`
    - List guilds: `docker run tyrrrz/discordchatexporter:stable guilds`
    - List channels: `docker run tyrrrz/discordchatexporter:stable channels --guild ###ID###`

## Processing tools

- [jq](https://stedolan.github.io/jq/)
    - Example: `jq -j -M --stream -f discord1.jq` [discord1.jq](https://gist.github.com/willsheppard/f9b7cc9b130784ffd7bd8f144cf892f8)

## Techniques

### Extract from chrome devtools

Good for: Structured data

- The network tab shows XHR requests being made 

## Discussion

- If an archive of data is made, and that data cannot be viewed in a way similar to its original presentation, then the average person can't view it at all. It may as well not exist for that person. You might retort "A viewer program could be built". But if that viewer program doesn't yet exist, then the data still can't be viewed. It's a Schroedinger's archive.

## Communities

- https://www.reddit.com/r/DataHoarder

## Similar projects

- https://github.com/igorbarinov/awesome-data-engineering
- https://github.com/lorien/awesome-web-scraping
