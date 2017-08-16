---
layout: post
title: And that's how Telegram saved the day
date: 2017-04-30T08:22:04.000Z
categories: technology
status: published
description: This posts talks about how Telegram bots can be used for Notifications.
display: true
tags:
  - devops
  - telegram
---

This story is based on the challenges faced during the setup of infrastructure, its maintenance and monitoring.

We recently added monitoring tool Grafana which helped us to get the KPIs of our applications running all over the infrastructure at one place. It was doing a great job and we added more and more parameters to it.

Whenever we had any issues in servers or if traffic went down significantly we got alerts and we can check what went wrong and can start fixing it within minutes. These alerts were through an email with all the details which we get for that alert. These email credentials were of Amazon SES SMTP Credentials with our registered domain.

In this chat bots generation majority of the people don't check emails that often. So as we developers were having problem checking mails and stuff we created a Telegram bot for our internal notification purposes and which sends messages in case of alert. Problem with that is we need a middle communication or parser which converts the alert information coming from Grafana to match the one supported by Telegram bot API but we were bit busy so we stood with just alert and not the full translation program. So when there's an alert we get a message from that notification bot saying "Alert!" that's it. In programmers language it was latch (toggle) kind of thing. If we get it once it's a problem then after some time we get it again it's says ok. So it was pretty simple if we get an alert we go to mail and check what went wrong and try to fix it.

Now let's recall that slow Friday, that day we were busy as usual with meetings and internal discussions. Suddenly after some time an alert triggered and we checked that significantly the traffic was going down. We didn't had any choice but to down the systems. But we didn't got any mails related to this alert just a dip which we can see in Grafana UI and Telegram bot's message.

We found out our cloud hosting provider had sent us many mails about our account expiration but all went to spam.

> Update (August 3, 2017): Recently Pavel Durov in his Telegram Channel also wrote something similar. It was not anything related to Notifications or stuff just comparing Email and Telegram. Following is the Text from the Channel quoted below

> "In addition to meeting local coders and early adopters of Telegram, I had a lunch with Mr. Rudiantara, the Minister of Communication of Indonesia. Our previous attempts to connect with Mr. Rudi failed because of unreceived e-mails (**e-mail is unreliable – let us all switch to Telegram!**), but in the end it was all for the best since we managed to establish a great personal connection." (Ref: [Message](https://t.me/durov/54))
