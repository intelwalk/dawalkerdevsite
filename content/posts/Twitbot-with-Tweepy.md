---
title: "Twitbot With Tweepy"
date: 2020-08-23T20:58:42-05:00
draft: false
image: "pythontweepy.jpg"
tags: [
	"python",
	"development",
    "howto",
    "tweepy"
]
categories: ["development",
                "howto",
                "python"]
---

# <center>Creating A TweetBot With Tweepy</center>

If you want to create a twitter bot you can easily with python and tweepy. It takes very little time to get one setup. We will be using two files for this example. Our python file wil be called twitbot.py and our file full of links (or whatever you want to tweet) in our list.txt file in the same directory.

Let's start this off with our python file named twitbot.py. First of all you need to make sure the tweepy library is installed.

```python
pip install tweepy
```

You will need to import the tweepy, sys, and time libraries in your script.

```python
import tweepy, time, sys
```

After this you need to make sure you have your application setup with your consumer/access keys and consumer/access secret keys. I won't be going over that unless I get some feedback that someone needs to see it actually done.

```python
CONSUMER_KEY = 'xxxxxxxxxxxxxxxx'#keep the quotes, replace this with your consumer key
CONSUMER_SECRET = 'xxxxxxxxxxxxx'#keep the quotes, replace this with your consumer secret key
ACCESS_KEY = 'xxxxxxxxxxxxxxxxxx'#keep the quotes, replace this with your access token
ACCESS_SECRET = 'xxxxxxxxxxxxxxx'#keep the quotes, replace this with your access token secret
```

Since you have your keys and secret keys setup you can get on with the next part and that is setting the authentication handler you will use with the library.

```python
auth = tweepy.OAuthHandler(CONSUMER_KEY, CONSUMER_SECRET)
```

We will give it your access token with this line.

```python
auth.set_access_token(ACCESS_KEY, ACCESS_SECRET)
```

Now we have the bearer token and ready to go!

```python
api = tweepy.API(auth)
```

Now we will switch to a file we use to keep a curated list of links with articles or videos I want to share in our twitter bot called list.txt and just have each link a seperate line in the file. Like this:

```text
Elixir Install How To www.youtube.com/video/GSpdBi6ow2E #elixir #programming #development
Binding An Orange R615x To A Spektrum DX5e www.youtube.com/video/Er0GeSj0cNw #RC #R615x #DX5e #Spektrum
Python Stack How-To www.youtube.com/video/BwCeGUEn1M8 #python #stack #programming #development
GoPro Studio Install Windows 10 www.youtube.com/video/DCgFq_foiXQ #gopro #goprostudio
Raspberry Pi Raspian Startup www.youtube.com/video/kLuGU77U4EA #python #RaspberryPi #pi #raspian
```

Now back to our twitboy.py file. When we run the twitbot.py script we will give it an arguement of the file that has the list of links in it as an arguement like this:

```python
./twitbot.py list.txt
```

We have to take the arg in so we know what the file is called

```python
argfile = str(sys.argv[1])
```

We have to open the files and read in all of the lines and then close the file too!

```python
filename=open(argfile,'r')
f=filename.readlines()
filename.close()
```

All thats left is to iterate over all the lines we've read in and update our status on twitter as well

```python
for line in f:
    api.update_status(status=line)
    time.sleep(180)#Tweet every 15 minutes
```

And there you have it you can check out the source code at https://github.com/intelwalk/python-twitbot

Thanks for checking this article out!
