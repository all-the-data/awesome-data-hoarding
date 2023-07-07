# awesome-data-hoarding

A concise cheat-sheet of commands and tools for scraping, saving, hoarding, archiving, collecting, organising and browsing data.

Inspired by Reddit's [/r/DataHoarder](https://www.reddit.com/r/DataHoarder/)

## Quick reference

Which archiving tool should you choose for each web service?

- Amazon Video: Unknown. Check torrents instead.

- BBC iPlayer: [youtube-dl](https://youtube-dl.org/) / [yt-dlp](https://github.com/yt-dlp/yt-dlp)

- Discord: DiscordChatExporter (see below for notes)

- Mediawiki website: Native dump using `/wiki/Special:AllPages` and `/wiki/Special:Export`.

- Netflix: Unknown. Check torrents instead.

- Reddit: [Bulk-Downloader-For-Reddit](https://github.com/aliparlakci/bulk-downloader-for-reddit) / [BDFRX](https://github.com/OMEGARAZER/bulk-downloader-for-reddit-x#differences-from-bdfr) / [Gallery-DL](https://github.com/mikf/gallery-dl) / [RipMe](https://github.com/RipMeApp2/ripme).
  - Historial data dumps: [the-eye](https://the-eye.eu/redarcs/) / [torrents](https://academictorrents.com/userdetails.php?id=9863)

- SoundCloud: [youtube-dl](https://youtube-dl.org/) / [yt-dlp](https://github.com/yt-dlp/yt-dlp)

- Tumblr: [TumblThreeApp](https://github.com/TumblThreeApp/TumblThree) (Windows). Viewers: [1](https://github.com/jacob-pro/tumbl-three-viewer), [2](https://github.com/willsheppard/random-scripts/blob/master/TumblThree_BackupViewer.html).

- Twitter: [ThreadReaderApp](https://threadreaderapp.com/)

- Torrents: Use [unblockit](https://www.google.com/search?q=unblockit) for a list of torrent sites. Official [Twitter](https://twitter.com/thepirateproxy) / [Reddit](https://www.reddit.com/r/Unblockit/).

- Private torrent trackers: Might contain any TV or movie ever broadcat. It can be difficult to get an invite, and you may need to maintain an upload ratio.

- Websites generally: wget, httrack or [ArchiveBot](https://wiki.archiveteam.org/index.php?title=ArchiveBot).

- Youtube video/music: [youtube-dl](https://youtube-dl.org/) (see below for notes) / [yt-dlp](https://github.com/yt-dlp/yt-dlp)

## Scraping tools

Details of precise sets of commands.

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

- Mediawiki
    - For an XML dump containing wikitext...
    - Copy names of pages from `/wiki/Special:AllPages`...
    - Paste into `/wiki/Special:Export`
    - (optional) Parse resulting wikitext with [mwparserfromhell](https://github.com/earwig/mwparserfromhell).

- [youtube-dl](https://yt-dl.org) / [yt-dlp](https://github.com/yt-dlp/yt-dlp)
  - Video

```
yt-dlp \
    --ignore-errors \
    --format 'bestvideo[ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]/best' \
    --output "%(playlist_title)s/%(title)s.%(ext)s" \
    --throttled-rate 10K \
    ###URL###
```

  - Audio

```
yt-dlp \
    --ignore-errors \
    --extract-audio \
    --audio-quality 0 \
    --audio-format mp3 \
    --prefer-ffmpeg \
    --output "%(playlist_title)s/%(artist)s - %(title)s.%(ext)s" \
    --throttled-rate 10K \
    ###URL###
```

  - Audio album playlist

```
yt-dlp \
    ...etc... \
    --output "%(artist)s - %(album)s/%(artist)s - %(album)s - %(playlist_index)02d - %(track)s.%(ext)s" \
    ###URL###
```

  - Video playlist

```
yt-dlp \
    ...etc... \
    --output "%(playlist_title)s/%(playlist_index)03d - %(artist)s - %(title)s.%(ext)s" \
    ###URL###
```

  - Multiple playlists

```
for URL in $(cat list)
do
    yt-dlp ...etc... "$URL"
done
```

- [DiscordChatExporter](https://github.com/Tyrrrz/DiscordChatExporter) + excellent [wiki](https://github.com/Tyrrrz/DiscordChatExporter/wiki)
    - Example: `docker run --rm -v /var/www/zaphod/adhd:/app/out tyrrrz/discordchatexporter:stable export --channel ###ID### --token ###SECRET### --format Json`
    - List guilds: `docker run tyrrrz/discordchatexporter:stable guilds`
    - List channels: `docker run tyrrrz/discordchatexporter:stable channels --guild ###ID###`

## Processing tools

- [jq](https://stedolan.github.io/jq/)
    - Example: `jq -j -M --stream -f discord1.jq` [discord1.jq](https://gist.github.com/willsheppard/f9b7cc9b130784ffd7bd8f144cf892f8)

- [XPath Helper](https://chrome.google.com/webstore/detail/xpath-helper/hgimnogjllphhhkhlmebbmlgjoejdpjl)
    - Example: Ctrl-Shift-X (or Command-Shift-X on Mac)

- [AutoHAR](https://github.com/Aloisius/autohar)

- [HAR Recorder](https://chrome.google.com/webstore/detail/har-recorder/emfabjnfjiknifjlfpjobbecfepplhkd)

## Techniques

### Extract playlist data from YouTube and YT Music

Input: https://music.youtube.com/library/playlists
Goal: Extract a list of playlists suitable for feeding to youtube-dl / yt-dlp

These are all equivalent ways to achieve the same thing:
1. Chrome: Save As | Web Page, HTML Only --> doesn't work, empty page
1. Chrome: Save As | Web page, Single File --> works, full HTML, embeds images, uses "quoted printable encoding", i.e. `=` becomes `=3D`
1. Chrome: Save As | Web page, Complete --> works, full HTML, not encoded, saves album/playlist covers as image files.
1. Chrome: DevTools | Elements | <body> | right-click | Copy | Copy element | Paste into text editor --> works, full HTML
1. Chrome: Extensions | XPath Helper | Ctrl-Shift-X | Hover over element | Shift | Edit XPath to remove e.g. `[409]` | Append `/@href` --> works, list of URLs
1. Chrome: DevTools | Console | [<load jquery>](https://stackoverflow.com/a/7474386) | [<XPath add-in>](https://stackoverflow.com/a/20495940) | `$(document).xpathEvaluate('//body/div/foo')`
1. Chrome: DevTools | Elements | right-click | Copy | Copy JS | (paste into console and edit - see snippet below)
1. Chrome: Extensions | AutoHAR | chrome --auto-open-devtools-for-tabs | ...etc
1. Chrome: DevTools | Network | Filter | Fetch/XHR | https://music.youtube.com/youtubei/v1/browse/...etc... | (a) Save all as HAR with content, (b) (down-arrow near top-right) Export HAR... 
1. (Idea) Headless chrome + puppeteer or playwright

Javascript snippet:
    
```
items = document.querySelectorAll("#items > ytmusic-two-row-item-renderer");
items.forEach((item) => {
    drill = item.querySelector("div.details.style-scope.ytmusic-two-row-item-renderer");
    span = drill.querySelector('span > yt-formatted-string > span:nth-child(3)');
    if (! span) { return };
    console.log(
        drill.querySelector('a').toString()
        + "    " + span.innerHTML
        + "    " + drill.querySelector('a').text
    );
});
```

Shorter snippet:
```
var output = '';
document.querySelectorAll("h3 > div > div > a").forEach((item) => { output += item.text + "\n"; });
console.log(output);
console.save(output);
```

[Save data out of console](https://stackoverflow.com/questions/41032565/how-to-copy-the-objects-from-chrome-console-window) via clipboard or writing a file (provides `console.save()` command.

### Case studies

- [naive-slack-scraper](). Hypothetical code that cannot exist, as it potentially wouldn't follow terms of service.
- [pokemon-data](https://github.com/pokemon-names/pokemon-data/blob/main/data/README.md). jq examples.

## Discussion

- If an archive of data is made, and that data cannot be viewed reasonably easily in a way similar to its original presentation by a person on the street, then it can be considered not to be viewable at all. It may as well not exist for public purposes. A possible retort is to assert "A viewer program could be built". But if that viewer program doesn't yet exist, then the data still can't be viewed. It's a Schroedinger's archive.

## Communities

- https://www.reddit.com/r/DataHoarder
- https://github.com/ArchiveBox/ArchiveBox/wiki/Web-Archiving-Community

## Similar projects

- https://github.com/iipc/awesome-web-archiving
- https://github.com/lorien/awesome-web-scraping
- https://github.com/igorbarinov/awesome-data-engineering
