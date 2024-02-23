# RSS feed scraper and parser prototypes

As of February 2024, these are a couple examples of related programs used to download RSS feeds quickly, then to parse them.

The Messenger scraper was the first. Data from that is at least temporarily available at https://rebrand.ly/the-messenger-archives.

Included in there was an attempt to begin parsing from snapshots of the web site's index pages (like, /news or /author/some-guy). That was abandoned when Zack Whittaker (@zackwhittaker@mastodon.social) observed The Messenger's stories were gone but the RSS feeds were still up. The RSS feeds even had the text of the stories. (It was missing secondary authors' names, alas.)

Just three weeks later, Vice employees discovered their email stopped working, and within hours were told they would be losing their jobs. There was an RSS feed  The Messenger's RSS parser worked almost perfectly for Vice's -- just needed the date format cleaned up. The resulting data is temporarily available at https://rebrand.ly/vice-archives.

## How this works

It likely won't work for you! It can be an instructive tale, or more likely, a cautionary tale, but there may be some stuff here you can adapt.

The two sites handled pagination differently, but the structure was nearly identical.

Basic approach:

Find the RSS feed if there is one -- if there is not, none of this will help.

Figure out the pagination schema.

Find the last page. For Vice it was basically, OK, can I find page 1000? 5000? 10000? 20000? 30000? 40000? No, too far. 35000? No. 32000? 31000? 31500? 31200?

Use Python to create a directory for these RSS files. Use Python to check for existing files. You can try to download using `requests` but you probably don't have that kind of time. Revisions to these scripts wound up creating a simple text file to be read by `aria2`, a stellar command-line program that allows you to download in bulk, e.g., `aria2c -x 16 -i rss-to-do.txt` and boom, you're downloading or at least requesting 16 files at a time.

Then parse. Vice's parser just needed a date format change from The Messenger's.

Then I found out Vice's data files were too big for Google Sheets to open; I tried breaking them into five-year chunks and *that* went badly; the late 2010s contained twice as much data as the other dozen years combined. I ultimately settled on single-year chunks but built an author index. You can see disabled code and the final code in those scripts. Github's previews simply don't show the disabled code but if you open them in Jupyter you will see them with a different indent.

## But why did you ...

These were both rush jobs, and the scripts show it. But, again, maybe there's something useful in here, if only as a cautionary tale.
