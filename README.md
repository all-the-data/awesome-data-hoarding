# awesome-data-hoarding

A collection of code and tools for scraping, hoarding, organising and browsing data

Inspired by Reddit's /r/DataHoarder

## Scraping tools

### General purpose

- [wget](https://www.gnu.org/software/wget/manual/wget.html)
    - Example: `wget --mirror --convert-links --adjust-extension --page-requisites --span-hosts -U Mozilla -e robots=off --no-cookies -D www.example.com,files.example.com,images.example.com http://www.example.com/`

- [StreamRipper](http://streamripper.sourceforge.net)
    - Example: `streamripper ###URL### -u "FreeAmp/2.x" -q -l 86400`

- [Chrome DevTools](https://developer.chrome.com/docs/devtools)
    - [network tab](https://developer.chrome.com/docs/devtools/network/reference) - track requests
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

- Check the Chrome 

## Communities

- https://www.reddit.com/r/DataHoarder

## Similar projects

- https://github.com/igorbarinov/awesome-data-engineering
- https://github.com/lorien/awesome-web-scraping
