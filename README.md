# teletext-twitter
Reads your Twitter timeline or searches for tweets and turns them into teletext pages for your Raspberry Pi

![Screenshot](https://i.imgur.com/xwnfUw3.jpg "Screenshot of output")

## What is this?
Useless, for a start.. But if you've ever had the desire to read your tweets by pushing the teletext button on your remote and keying a page number then this is the package for you! Retro chic for the social media generation!

## Installation
After cloning the repository you'll need to install the Python-Twitter wrapper with pip:
`pip install python-twitter`

Before you get this up and running there are two things you need to do first:

1) Connect your Raspberry Pi to a TV over composite using a cable with the correct pin-out. I bought mine from The Pi Hut: https://thepihut.com/products/adafruit-a-v-and-rca-composite-video-audio-cable-for-raspberry-pi
2) Install the VBIT2 and raspi-teletext apps and make sure they are outputting teletext data to your TV
..* raspi-teletext (Alistair Buxton): https://github.com/ali1234/raspi-teletext - Only PAL is supported by raspi-teletext
..* VBIT2 (Peter Kwan): https://github.com/peterkvt80/vbit2

After getting these up and running the last thing to do is to rename config.py-default to config.py and get your Twitter access tokens to store in the file. You can find a good guide for doing this here: https://iag.me/socialmedia/how-to-create-a-twitter-app-in-8-easy-steps/

You will need to pick a unique name for the app. Which is annoying. Pick anything you want that isn't taken. Maybe add your name at the end.

Change the tti_path line too if you've changed your Teefax location.

When you've setup your config.py you can run the script like this example that grabs your home timeline:

`python3 teletext-twitter -t` (add -h to show options - also listed below)

The script will constantly update a spare page (153 - chosen because it used to be used for this purpose on Teefax in the past) in the main teletext folder (which defaults in VBIT2 to /home/pi/teletext/).

All of the files in that folder are sent across to the TV every so often, therefore the script constantly overwrites it with new tweets (up to 5 - space permitting!) so that it will update on your screen.

## Notes
* At this moment in time the script reads 5 tweets. Further versions will improve on this by writing multiple tweets, possibly in subpages :-O
* New tweets are grabbed every 60 seconds by default. This is configurable with the -d option, but you do have be aware of the Twitter API limits.
* Emoji stripping is now included. Mostly.
* Due to the limited teletext character set substitutions are implemented. Things like replacing underscores with hyphens and also making sure # works correctly. You also have to replace curly apostrophes with straight ones, as that's all the specification allows

Apart from those notes, things should work ok. Have fun, turn back the clock, and if you genuinely use this for anything please let me know (also you're mental/cool).

### -h/--help output

<pre>
usage: teletext-twitter.py [-h] [-t] [-s] [-q QUERY] [-d DELAY] [-v] [-Q]

Reads your Twitter timeline and turns it into teletext pages for your Raspberry Pi.

optional arguments:
  -h, --help            show this help message and exit
  -t, --timeline        download your latest home timeline
  -s, --search          specify a term to search for
  -q QUERY, --query QUERY
                        a search query, hashtags supported if you put quotes
                        around the string
  -d DELAY, --delay DELAY
                        seconds between timeline scrapes (minimum is 60
                        seconds - lower values have no effect)
  -v, --version         show program's version number and exit
  -Q, --quiet           suppresses all output to the terminal except warnings
                        and errors
</pre>
