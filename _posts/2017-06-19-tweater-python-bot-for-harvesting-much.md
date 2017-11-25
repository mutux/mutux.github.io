---
layout: post
title: 'TwEater: A Python bot for scraping more tweets and comments'
date: '2017-06-19T14:52:00.002-07:00'
author: mutux
tags:
modified_time: '2017-06-19T14:52:24.417-07:00'
blogger_id: tag:blogger.com,1999:blog-7424095106938843032.post-7070822754424735799
blogger_orig_url: http://mutux.blogspot.com/2017/06/tweater-python-bot-for-harvesting-much.html
comment_id: 2
---

Today, I submitted my way of scraping tweets on Twitter - **[TwEater](https://github.com/mutux/TwEater)** on GitHub.

It initially aims at collecting tweets and replies from Twitter for my project of Opinion Mining. After costing some time on trying official Twitter API, I was frustrated to find out that Twitter dose not intend to support researchers to collect enough data any more. Only tweets posted in the past 7 days, at most 3200 tweets per search are provided with official Twitter API.

But we need more, so here comes this bot **TwEater**. Except for the basic attributes of tweets like text, time, user, and id, it can also collect **replies**, mentions, hashtags, links, and **emojis**. The most important thing is that it enables access of tweets without limitations of **`time`** and **`amount`**.

Which also means the data scraped by this bot can support many different text mining tasks, such as sentiment analysis, chatting bot, tweet ranking, topic detection & tracking, social network analysis and so on.

So go check it out, and have fun! For research only please!

# Example

After importing `TwOrder` and `TwEater` from `tweater`, its usage is like this:

{% highlight python linenos %}
# Write tweets batch to a file in folder dir
def digest_2_file(tweets, dir):
    fn = dir + '/test.json'
    with open(fn, 'w') as f:
        json.dump(tweets, f)


if __name__ == "__main__":
    # Initialize the parameters
    TwOrder.order('order.conf') # TwOrder.order(user='BarackObama')
    # Write tweets to json file
    TwEater.eatTweets(digest_2_file, 'test')
{% endhighlight %}

# Reference
+ [Github Repository](https://github.com/mutux/TwEater)
