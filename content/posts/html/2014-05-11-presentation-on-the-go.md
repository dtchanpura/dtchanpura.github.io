---
layout: post
title: "Presenting using reveal.js"
categories: html
date: 2014-05-11 12:00:00
status: published
description: "Presentations are not just the way of explaining but also the way of making an impression. This post is all about that an introduction to simple yet powerful <i>slid.es</i>"
display: true
aliases: [/html/2014/05/11/presentation-on-the-go.html]
---
Being in a person in a field where he has to present his thoughts and his work. Powerpoint or Impress (from Apache OpenOffice.org or LibreOffice) are pretty good but it needs one of them for presenting. But what if we can make things present without them.

The trio HTML5, CSS3 and JS have really big impact on this generation of Internet/Web. The same way it has converted the web browser not only into HTML parser like showing or displaying websites but a well designed web display and parser.

I just came across [Slides](http://slid.es). Which used the presentation javascript called [reveal.js](http://hakim.se/projects/reveal-js) a project by Hakim El Hattab. The best use of CSS i have seen till now.

Now that's simple we just have to clone the framework add the presentation in form of html file and then it will parse most of the required controls for slide-show.

The following is the screenshot of the sample slide but you can surely see the demos at slid.es which just sign up and start creating great designs.

<a href="http://lab.hakim.se/reveal-js/#/"><img src="/images/reveal.png" width="100%" /></a>

It has a great tutorials but I just prefer to keep it simple so i cloned the latest from github by

```sh
git clone https://github.com/hakimel/reveal.js.git
```

For your presentation just type-in or edit the sections. Even if one is not good at html he/she can just go to slid.es and make use of the online editor and export in form of html code. Just add the copied code inside the `<div class="reveal">...</div>`

That's it and it does have many features like themes, effects and insertion of medias, etc. It is quite useful who has most of the usage while presenting or demostrating in browsers.
