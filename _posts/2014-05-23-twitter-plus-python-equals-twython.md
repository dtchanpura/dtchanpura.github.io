---
layout: post
title: "Twitter + Python + Raspberry Pi"
categories: raspi
date: 2014-05-23 14:11:59
status: published
description: "A simple LCD interfacing with Raspberry Pi and one its application to display the tweets having a #hashtag."
display: true
---

In earlier post we discussed about GPIOs and now we will use them with interfacing LCD of 16x2 (I am using 20x4) for displaying Tweets of some #hashtag (#DCPrimeRPi) or @someuser (@dtchanpura) on LCD. I am using a module named Twython which I came across while searching for a tweetbot. Well I did find a Tweeting Bot but never made that on my Raspberry Pi just reinvented and made a small display to show the thoughts about it.

<div id="container"><img src="/images/tweet_on_lcd.jpg" /></div>

This post will take you to the a way how you can use two different modules and make use of both at once.

##LCD:

For interfacing LCD with Raspberry Pi we have connected it the following manner.

As seen the LCD is interfaced and it will be able to show what we want. Now comes the "Hard Part" (For me as I belong to Electronics and not Computer so Connections are easy) which is coding. Fortunately there is a Library available by Adafruit which allows to use a character LCD. I have modified it a little bit and you can have it from <a href="http://github.com/dtchanpura/DCPrimeRPi">github.com/dtchanpura/DCPrimeRPi</a>. This is having an ability to use long sentences without special characters whose ASCII goes above some number as we are using LCD in 4bit mode.

Code is quite simple as for start to display something we just need a place where we can give the variable name of a string. So following is hello world code for a LCD display. (Cloned from my Repo DCPrimeRPi)

###CODE:

```python
#!/usr/bin/env python

from lcdDisplay import lcdDisplay
lcd = lcdDisplay() #Object named lcd for
#using all the functions of lcdDisplay.py

lcd.begin(20,3)

lcd.clear()
i=0
lcd.longmessage("The Quick Brown\
Fox Jumps over a Lazy Dog.The Quick\
Brown Fox Jumps over a Lazy Dog.The\
Quick Brown Fox Jumps over a Lazy Dog.\
The Quick Brown Fox Jumps over a.")
```

So here we can see I have used only around 160 characters (Limit of Tweet.) and it does display it better but the well known types of LCDs are having only 16x2 = 32 characters. So how to show.. is a big question here.

To show more characters I looked upon and thought how in a movie the end-titles are shown. "One by One." That has been used here. Now to do so I have made a new function (the change in Adafruit's Module) now this needs to go deep. Like we have to see how is the display function made. Now lets have a look on the display function which is `lcd.message()`

```python
def message(self, text):
        """ Send string to LCD. Newline wraps to second line"""

        for char in text:
            if char == '\n':
                self.write4bits(0xC0) # next line
            else:
                self.write4bits(ord(char),True)

```

This code has a Defect like it just displays the static text or some dynamic test limited to 32 characters but ours is quite more. Like <i>@dtchanpura: #DCPrimeRPi Tweeting with this hash tag for some reason</i> Considering this for making it fit in 32 characters we need to split it in blocks of 32. But in such a way that there's a repetition from change in screen. Like as follows:

```
@dtchanpura:
\#DCPrimeRPi Twee
```
```
\#DCPrimeRPi Twee
ting with this ha
```
```
sh tag for some r
eason
```

So to do so we need to modify the program a bit (or a lot). So i have added a new function which is just for displaying the long messages and the function is named `longmessage()`

For this we will divide the string into mainly 4 different parts as we can see in above tweet its 3 parts. So to divide we will split the string with 16 characters at once which are then repeated in another list as shown below.

```['#DCPrimeRPi Twee','ting with this ha','sh tag for some r','eason']```

This will show us that use only two lines or four lines at once. There are many people who are using 4 line LCDs and I was one of them I started debugging and editting the code so that I can show the whole 160 characters in some scroll effect without missing any line.

The change is shown below.

```python
def longmessage(self, text):
  # text=str(text)
  if len(text)<80:
    text=text+' '\*(80-len(text))
    li=0
    for char in text:
      if char=='\n':
        text = text[:li] + ' ' + text[li+1:]
        li+=1
        text1=self.splitnumv2(4, self.splitnum(20,text))
        print text1
        text1=text1[:len(text1)-3]
        """ Send string to LCD. Newline wraps to second line"""
        for screen in text1:
          self.home()
          l=0
          \# self.clear()
          for line in screen:
            line=line + ' ' * (20-len(line))
            for char in line:
              l+=1
              self.write4bits(ord(char),True)
              if l==20:
                self.write4bits(self.LCD_LINE2) # 2nd line
              elif l==40:
                self.write4bits(self.LCD_LINE3) # 3rd line # changed
              elif l==60:
                self.write4bits(self.LCD_LINE4) # 4th line #changed
          sleep(4)
```

This code has just one change it will be making a group of four and then removing one from start and append new line at the end. In this way we call this group of four lines a screen and this screen stays for 4 secs as mentioned in the last line of the code.

##Twitter:

The half part was done for displaying but what to display? Twitter is useful and also well known for the short messages that are broadcast so we here will be using that API given by Twitter and ported to Python by the name Twython.

To use it we just need a Twitter account and some application which you can easily get from [http://apps.twitter.com](https://apps.twitter.com/app/new).

###Installing Twython on Raspi:

For installing we need to install the <code>python-setuptools</code> like <code>pip</code> to do so we just type in the following commands. So by using <code>pip</code> we install the whole API of Twitter which has been already ported to Python.

```sh
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install python-setuptools
sudo easy_install pip
sudo pip install twython
```

###Registering Twitter Application:

Go to <a href="https://apps.twitter.com/">http://apps.twitter.com</a> sign in or sign up for creating a new application. This will give following kind of screen which needs the name and its url if used. Fill up the full form and then it will ask for the authentication and rights. It simply means is this app been used for read purpose or also for writing i.e. just reading tweets or for tweeting too. It can be used if someone wants to share the output to some command and directly share on Twitter.
<div id="container"><img src="/images/newapp_twitter.png" /></div>
Now after creating the app you will see the authentication keys or access tokens. Keep them safe they are important and secret. Also we will need them for the Pi.
<div id="container"><img src="/images/access_token.jpg" /></div>

###Summing things up...

After installing twython and creating an app we need to add our code to Pi.

```sh
mkdir TweetMon && cd TweetMon
git clone http://github.com/dtchanpura/DCPrimeRPi.git -b CLCD
```
This repository has all the prerequisites for this project. The thing needed to add in it is the access codes so for that create a new file with path in the directory `DCPrimeRPi/CLCD_python/twitter_keys.py`

Now its all done the code has the comments and is self explanatory. It does search for a hashtag which uses the command api.search and then returns the values to `lcd.longMessage()`.

So at the end it may look like this...

<div id="container"><img src="/images/tweet_on_lcd.jpg" /></div>

End of Post

References:

http://www.makeuseof.com/tag/how-to-build-a-raspberry-pi-twitter-bot/

Images from adafruit/flickr
