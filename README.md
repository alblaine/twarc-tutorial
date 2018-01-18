Getting Data from Twitter: A twarc tutorial
=====

[![Build Status](https://secure.travis-ci.org/DocNow/twarc.png)](http://travis-ci.org/DocNow/twarc)

twarc is a command line tool and Python library for archiving Twitter JSON data.
Each tweet is represented as a JSON object that is
[exactly](https://dev.twitter.com/overview/api/tweets) what was returned from
the Twitter API.  Tweets are stored as [line-oriented JSON](https://en.wikipedia.org/wiki/JSON_Streaming#Line_delimited_JSON).  Twarc will handle
Twitter API's [rate limits](https://dev.twitter.com/rest/public/rate-limiting)
for you. In addition to letting you collect tweets Twarc can also help you
collect users, trends and hydrate tweet ids.

twarc was developed as part of the [Documenting the Now](http://www.docnow.io)
project which was funded by the [Mellon Foundation](https://mellon.org/).

## Step 1: Download this Repository
Click on the **Clone or Download** button. Select **Download Zip**.

Unzip the directory on your computer. Move this directory to your Documents folder.

You should now have a folder on your Documents directory called twarc-tutorial-master.


## Step 2: Create a Twitter Account and and Register an Application

You will need a Twitter account and a registered Twitter application at [apps.twitter.com](http://apps.twitter.com).

* Sign in to [apps.twitter.com](http://apps.twitter.com) with your Twitter Account.
* Click **Create New App** button
* Give your App a unique title and write an concise and honest description about what it is.
* Include a link where you can lean more about your app. You can use the URL to this repository for now, but if you make a project out of this tweet harvest, you will want to change it to your own URL. 
* Leave the Callback URL window empty.
* Make sure to read the [Twitter Developer Terms](https://developer.twitter.com/en/developer-terms/agreement-and-policy) before agreeing to them.
* Once you've created your application, click to generate an access token and access token secret.
* You should now have the following 4 things: consumer key, consumer secret, access token and access token secret. Leave the window open because you'll need this info to configure twarc.

## Step 3: Install Python 3+ (if you don't have it already)

1. Open a command prompt window on your computer. On a Mac, this is your Terminal application (located in the Utilities folder). On Windows, is the command prompt (right click on Start button).
2. Type `python --version` to see what version of Python your computer recognizes
3. If you don't have Python 3+, download and install [Python 3](http://python.org/download)
4. After installing, type `python3 ` in the command prompt.
5. Now, type `pip3 install twarc` to install twarc from `pip`. (if upgrading, type: `pip3 install --upgrade twarc`)

Troubleshooting:

* `pip3: command not found` -- this means that your computer can't find pip3. This package installed with python 3, so it should automatically be there. To manually install pip, [follow these directions](https://pip.pypa.io/en/stable/installing/).
* `permission denied` errors -- you may have trouble installing stuff into root directories depending on how your computer is configured. If you already know how to use the command line, you can go in and [change your file permissions](https://www.computerhope.com/unix/uchmod.htm) so that you have admin access to certain folders on your computer. Another way around this is to type `sudo` before commands, which will prompt you to enter your admin password. Example:
`sudo pip3 install twarc`

## Step 4: Navigate to Your Twarc directory
Open a terminal (or command line) application.

* Mac users: Go to your Applications Folder. Open **Utilities**. Now open the **Terminal**.

* Windows users: Click the lower-left **Start** button to open the Start Menu, and type **cmd** down in the white box. You should see "Command Prompt" appear in a list of results. Click to open.

* Once the terminal opens, you'll see a blank screen with a prompt at the top.

* Navigate to your twarc-tutorial folder by typing the following into the command prompt: `cd documents/twarc-tutorial-master` and hitting enter.

* Now, type `pwd` and hit enter to make sure you're in the twarc-tutorial-master folder.


## Step 5: Configure Twarc

Once you've got twarc installed and your your application keys from Twitter, you can tell twarc what they are with the `configure` command. On the command line, type:

`twarc configure`

This will store your credentials in a file called `.twarc` in your home
directory so you don't have to keep entering them in. If you would rather supply
them directly you can set them in the environment (`CONSUMER_KEY`,
`CONSUMER_SECRET`, `ACCESS_TOKEN`, `ACCESS_TOKEN_SECRET`) or using command line
options (`--consumer_key`, `--consumer_secret`, `--access_token`,
`--access_token_secret`).

## Step 6: Harvest Some Tweets

This section uses Twitter's [search/tweets](https://dev.twitter.com/rest/reference/get/search/tweets) to download *pre-existing* tweets matching a given query. If you just let it run, you'll get a large file of 100+ MB. We're only going to let it run a little while.

##### Run a Search

Let's harvest some tweets about the **flu**:

Type `twarc search flu > tweets.json`

Let it run a minute, and then type `CTRL + C` to stop the harvest

##### Explore Tweets

To see how many tweets you have, type:

`wc -l tweets.json`

To see what's in the file, type `cat tweets.json`

It's important to note that `search` will return tweets that are found **within a
7 day window** that Twitter's search API imposes. If this seems like a small
window, it is, but you may be interested in collecting tweets as they happen
using the `filter` and `sample` commands below.

The best way to get familiar with Twitter's search syntax is to experiment with
[Twitter's Advanced Search](https://twitter.com/search-advanced) and copy and
pasting the resulting query from the search box.


## Step 7: Remove RTs and Deduplicate
In the utils folder are some handy utilities you can use to process your file.

To remove retweets, type `utils/noretweets.py > noretweets.json`

To remove duplicate tweets, type
`utils/deduplicate.py noretweets.json > nodupes.json`

To remove potentially sensitive tweets (optional), type
`utils/sensitive.py nodupes.json > nosensitive.json`

To see how many tweets you have in any of these files, you can use the following command:

`wc -l`

## Step 8: Make a wordcloud
Type the following command:

`utils/wordcloud.py tweets.json > wordcloud.html`

If you go into your files from the graphical/windows interface (not the command line) and click on wordcloud.html, it will open in a browser and you'll see the wordcloud.

## Step 9: Create a tweet wall

Type `utils/wall.py  tweets.json > wall.html` to create an html file where the tweets will be displayed like cards in a browser window.

## Step 10: Take a subset of tweets harvested

The command below will save 100 tweets to a new file called search100.json

`head -100 nosensitive.json > search100.json`

## Step 11: Convert JSON to csv
Google search for "JSON to CSV converter"

Here's an example: https://json-csv.com/

Drop your search100.json file in here and convert your tweet file to a CSV (won't work with larger files)

After downloading your CSV file, you can open it in Excel.

## More Options
### JSONL
Twarc official documentation suggests saving files to .jsonl files. Example:

`twarc search '#blacklivesmatter > tweets.jsonl`

The challenge with `.jsonl` files is that they are harder to convert to other file types, such as CSV, without knowing how to code.

The **following** examples use `.jsonl` extensions, but can also be used with `.json` extensions.

### More Complex Searches

Here is a more
complicated query that searches for tweets containing either the
\#blacklivesmatter or #blm hashtags that were sent to deray.

  `twarc search '#blacklivesmatter OR #blm to:deray' > tweets.jsonl`

Twitter attempts to code the language of a tweet, and you can limit your search
to a particular language if you want:

  `twarc search '#blacklivesmatter' --lang fr > tweets.jsonl`

You can also search for tweets with a given location, for example tweets
mentioning *blacklivesmatter* that are 1 mile from the center of Ferguson,
Missouri:

  `twarc search blacklivesmatter --geocode 38.7442,-90.3054,1mi > tweets.jsonl`

If a search query isn't supplied when using `--geocode` you will get all tweets
relevant for that location and radius:

  `twarc search --geocode 38.7442,-90.3054,1mi > tweets.jsonl`

### Filter

The `filter` command will use Twitter's [statuses/filter](https://dev.twitter.com/streaming/reference/post/statuses/filter) API to collect tweets as they happen.

`twarc filter blacklivesmatter,blm > tweets.jsonl`

Please note that the syntax for the Twitter's track queries is slightly
different than what queries in their search API. So please consult the
documentation on how best to express the filter option you are using.

Use the `follow` command line argument if you would like to collect tweets from
a given user id as they happen. This includes retweets. For example this will
collect tweets and retweets from CNN:

    twarc filter --follow 759251 > tweets.jsonl

You can also collect tweets using a bounding box. Note: the leading dash needs
to be escaped in the bounding box or else it will be interpreted as a command
line argument!

    twarc filter --locations "\-74,40,-73,41" > tweets.jsonl


If you combine options they are OR'ed together. For example this will collect
tweets that use the blacklivesmatter or blm hashtags and also tweets from user
CNN:

    twarc filter blacklivesmatter,blm --follow 759251 > tweets.jsonl

### Sample

Use the `sample` command to listen to Twitter's [statuses/sample](https://dev.twitter.com/streaming/reference/get/statuses/sample) API for a "random" sample of recent public statuses.

    twarc sample > tweets.jsonl

### Dehydrate

The `dehydrate` command generates an id list from a file of tweets:

    twarc dehydrate tweets.jsonl > tweet-ids.txt

### Hydrate

Twarc's `hydrate` command will read a file of tweet identifiers and write out the tweet JSON for them using Twitter's [status/lookup](https://dev.twitter.com/rest/reference/get/statuses/lookup) API.

    twarc hydrate ids.txt > tweets.jsonl

Twitter API's [Terms of Service](https://dev.twitter.com/overview/terms/policy#6._Be_a_Good_Partner_to_Twitter) discourage people from making large amounts of raw Twitter data available on the Web.  The data can be used for research and archived for local use, but not shared with the world. Twitter does allow files of tweet identifiers to be shared, which can be useful when you would like to make a dataset of tweets available.  You can then use Twitter's API to *hydrate* the data, or to retrieve the full JSON for each identifier. This is particularly important for [verification](https://en.wikipedia.org/wiki/Reproducibility) of social media research.

### Users

The `users` command will return User metadata for the given screen names.

    twarc users deray,Nettaaaaaaaa > users.jsonl

You can also give it user ids:

    twarc users 1232134,1413213 > users.jsonl

If you want you can also use a file of user ids, which can be useful if you are
using the `followers` and `friends` commands below:

    twarc users ids.txt > users.jsonl

### Followers

The `followers` command  will use Twitter's [follower id API](https://dev.twitter.com/rest/reference/get/followers/ids) to collect the follower user ids for exactly one user screen name per request as specified as an argument:

    twarc followers deray > follower_ids.txt

The result will include exactly one user id per line. The response order is
reverse chronological, or most recent followers first.

### Friends

Like the `followers` command, the `friends` command will use Twitter's [friend id API](https://dev.twitter.com/rest/reference/get/friends/ids) to collect the friend user ids for exactly one user screen name per request as specified as an argument:

    twarc friends deray > friend_ids.txt

### Trends

The `trends` command lets you retrieve information from Twitter's API about trending hashtags. You need to supply a [Where On Earth](http://developer.yahoo.com/geo/geoplanet/) identifier (`woeid`) to indicate what trends you are interested in. For example here's how you can get the current trends for St Louis:

    twarc trends 2486982

Using a `woeid` of 1 will return trends for the entire planet:

    twarc trends 1

If you aren't sure what to use as a `woeid` just omit it and you will get a list
of all the places for which Twitter tracks trends:

    twarc trends

If you have a geo-location you can use it instead of the `woedid`.

    twarc trends 39.9062,-79.4679

Behind the scenes twarc will lookup the location using Twitter's [trends/closest](https://dev.twitter.com/rest/reference/get/trends/closest) API to find the nearest `woeid`.

### Timeline

The timeline command will use Twitter's [user timeline API](https://dev.twitter.com/rest/reference/get/statuses/user_timeline) to collect the most recent tweets posted by the user indicated by screen_name.

    twarc timeline deray > tweets.jsonl

You can also look up users using a user id:

    twarc timeline 12345 > tweets.jsonl

### Retweets and Replies

You can get retweets for a given tweet id like so:

    twarc retweets 824077910927691778 > retweets.jsonl

If you want to get the replies to a given tweet you can:

    twarc replies 824077910927691778 > replies.jsonl

Using the `--recursive` option will also fetch replies to the replies as well as
quotes.  This can take a long time to complete for a large thread because of
rate limiting by the search API.

    twarc replies 824077910927691778 --recursive

Unfortunately Twitter's API does not currently support getting replies to a
tweet. So twarc approximates it by using the search API. Since the search API
does not support getting tweets older than a week twarc can only get all the
replies to a tweet that have been sent in the last week.

To get the users that are on a list you can use the list URL with the
`listmembers` command:

    twarc listmembers https://twitter.com/edsu/lists/bots

## Use as a Library

If you want you can use twarc programmatically as a library to collect
tweets. You first need to create a `Twarc` instance (using your Twitter
credentials), and then use it to iterate through search results, filter
results or lookup results.

```python
from twarc import Twarc

t = Twarc(consumer_key, consumer_secret, access_token, access_token_secret)
for tweet in t.search("ferguson"):
    print(tweet["text"])
```

You can do the same for a filter stream of new tweets that match a track
keyword

```python
for tweet in t.filter(track="ferguson"):
    print(tweet["text"])
```

or location:

```python
for tweet in t.filter(locations="-74,40,-73,41"):
    print(tweet["text"])
```

or user ids:

```python
for tweet in t.filter(follow='12345,678910'):
    print(tweet["text"])
```

Similarly you can hydrate tweet identifiers by passing in a list of ids
or a generator:

```python
for tweet in t.hydrate(open('ids.txt')):
    print(tweet["text"])
```

## Utilities

In the utils directory there are some simple command line utilities for
working with the line-oriented JSON, like printing out the archived tweets as
text or html, extracting the usernames, referenced URLs, etc.  If you create a
script that you find handy please send a pull request.

When you've got some tweets you can create a rudimentary wall of them:

    % `utils/wall.py tweets.jsonl > tweets.html`

You can create a word cloud of tweets you collected about nasa:

    % `utils/wordcloud.py tweets.jsonl > wordcloud.html`

If you've collected some tweets using `replies` you can create a static D3
visualization of them with:

    % `utils/network.py tweets.jsonl tweets.html`

Optionally you can consolidate tweets by user, allowing you to see central accounts:

    % `utils/network.py --users tweets.jsonl tweets.html`

And if you want to use the network graph in a program like [Gephi](https://gephi.org/),
you can generate a GEXF file with the following:

    % `utils/network.py --users tweets.jsonl tweets.gexf`

gender.py is a filter which allows you to filter tweets based on a guess about
the gender of the author. So for example you can filter out all the tweets that
look like they were from women, and create a word cloud for them:

    % `utils/gender.py --gender female tweets.jsonl | utils/wordcloud.py > tweets-female.html`

You can output [GeoJSON](http://geojson.org/) from tweets where geo coordinates are available:

    % `utils/geojson.py tweets.jsonl > tweets.geojson`

Optionally you can export GeoJSON with centroids replacing bounding boxes:

    % `utils/geojson.py tweets.jsonl --centroid > tweets.geojson`

And if you do export GeoJSON with centroids, you can add some random fuzzing:

    % `utils/geojson.py tweets.jsonl --centroid --fuzz 0.01 > tweets.geojson`

To filter tweets by presence or absence of geo coordinates (or Place, see [API documentation](https://dev.twitter.com/overview/api/places)):

    % `utils/geofilter.py tweets.jsonl --yes-coordinates > tweets-with-geocoords.jsonl`
    % `cat tweets.jsonl | utils/geofilter.py --no-place > tweets-with-no-place.jsonl`

To filter tweets by a GeoJSON fence (requires [Shapely](https://github.com/Toblerity/Shapely)):

    % `utils/geofilter.py tweets.jsonl --fence limits.geojson > fenced-tweets.jsonl`
    % `cat tweets.jsonl | utils/geofilter.py --fence limits.geojson > fenced-tweets.jsonl`

If you suspect you have duplicate in your tweets you can dedupe them:

    % `utils/deduplicate.py tweets.jsonl > deduped.jsonl`

You can sort by ID, which is analogous to sorting by time:

    % `utils/sort_by_id.py tweets.jsonl > sorted.jsonl`

You can filter out all tweets before a certain date (for example, if a hashtag was used for another event before the one you're interested in):

    % `utils/filter_date.py --mindate 1-may-2014 tweets.jsonl > filtered.jsonl`

You can get an HTML list of the clients used:

    % `utils/source.py tweets.jsonl > sources.html`

If you want to remove the retweets:

    % `utils/noretweets.py tweets.jsonl > tweets_noretweets.jsonl`

Or unshorten urls (requires [unshrtn](https://github.com/edsu/unshrtn)):

    % `cat tweets.jsonl | utils/unshorten.py > unshortened.jsonl`

Once you unshorten your URLs you can get a ranked list of most-tweeted URLs:

    % `cat unshortened.jsonl | utils/urls.py | sort | uniq -c | sort -nr > urls.txt`

## twarc-report

Some further utility scripts to generate csv or json output suitable for
use with [D3.js](http://d3js.org/) visualizations are found in the
[twarc-report](https://github.com/pbinkley/twarc-report) project. The
util `directed.py`, formerly part of twarc, has moved to twarc-report as
`d3graph.py`.

Each script can also generate an html demo of a D3 visualization, e.g.
[timelines](https://wallandbinkley.com/twarc/bill10/) or a
[directed graph of retweets](https://wallandbinkley.com/twarc/bill10/directed-retweets.html).

[pt-BR]: https://github.com/DocNow/twarc/blob/master/README_pt_br.md
