---
layout: post
title: First Party tracking
date: 2017-08-01T08:22:04.000Z
lastmod: 2017-08-19T19:15:14.000Z
categories: technology
status: beta
description: >-
  This post talks about how third party tracking works and how it can be
  replaced with first party tracking (same domain tracking).
display: false
tags:
  - devops
  - tracking
  - privacy
aliases: [/technology/2017/08/01/first-party-tracking.html]
draft: true
---

First party tracking - I don't know if there's any word like first-party but I assumed the third-party tracking term came from Server as publisher and first party and Client as user and second party can be used alternatively and third party being other tracking partners to whom the publisher's are integrated with.

# Tracking and its usage

Tracking is mainly used for analytics and to answer some of the following questions: (without cookie)

- What kind of device people are using visiting this site?
  - Also Operating System, Device as Desktop or Mobile or more information like screen resolution.
- What pages are most visited?
  - Also what are the most traffic coming from as search or share.
- Which visitors are from which countries?
  - It may have drilled down to region/states or cities too.

Its the same as what we can see from [Google Analytics](https://analytics.google.com) and the services they provide.

Now if you want to integrate Google Analytics you just need to add a simple Javascript to all of your pages and one will be able to see all the details which you need to your dashboard.

To track all the users Google Analytics use a cookie and store the uniqueness of the user in it. This helps them to create another metric for analytics which is related to User's behavior and can answer following more questions: (with cookie)

- How many users are visiting again?
- How many new users are visiting every day?

So the earlier approach without cookie is basic tracking which is helpful for places where quantity matters more rather than quality. In simpler terms the first approach is mainly for those who need just the tracking to check how many number of users are hitting the site, to know what kind of users they are you need to move another approach.

Till this part you should what tracking/analytics can provide.

## Side effects

There are many things involved with this type of tracking. If the tracking is dumb i.e. not having information about user then it is having no more information than the site which user accesses. But if the tracking/analytics is intelligent like with cookies it may have more information about the user than the site (unless the site requires signing-in).

Now one may think what is the side effects of this type of intelligent tracking, at the end it is providing more insights to enhance the site experience. Read through the explanation.

### Data Ownership

As per the privacy policy the third party tracking data is shared to Analytics provider. Then this with cookies is compared with all different kind of sites data whom the user is visiting and unified to know how user behaves.

We have no idea where this data is shared or how much human interaction is done with it. So this can be the side effect as the data which belongs to us is stored somewhere we don't even know.

### Do not track

Do not track is sometimes good and sometimes bad. This can be really good example of **Trust**. We trust that by turning on the "Do not track" feature provided by browsers (or setting `DNT: 1` header in the request) the site or analytics provider will not track.

But as you can see what I highlighted here is **Trust**. We cannot trust them unless they have mentioned somewhere in their privacy policy. There can be publishers/sites which are following the DNT strictly.

To make sure you need to disable tracking everywhere I recommend [Privacy Badger](https://www.eff.org/privacybadger) for Desktop Browsers.

# First Party Tracking

Current implementation of these third-party tracking is they inject some Javascript tag and start tracking every request and process it real-time.

To avoid this kind of third-party tracking but still knowing answers to our questions which we need, I started adding this simple Javascript tag and a logger in Nginx which was a subdomain of this blog.

I know the infrastructure can cost for this type of implementation and that is the reason why publishers try to decrease the cost by going with different Analytics provider.

To simplify the things we can have a triangle with corners of simplicity, 1/cost and privacy. More you're towards privacy you go away from simplicity and increase in cost. Other thing what currently publishers have implemented is like lesser privacy and less cost which is near to simplicity and 1/cost.

## Approach

I tried to use simple things here for getting the user access log through Nginx.

This blog was hosted at Github and uses Github Pages to generate and build. Which is ok for a blog but I cannot get the log for these pages. So to track pages I started using a subdomain of the blog `px.dcpri.me` which has a sole purpose of tracking and loading Javascript.

## Code

To be added.
