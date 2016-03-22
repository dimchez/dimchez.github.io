+++
date = "2016-03-21T21:43:24+01:00"
draft = false
title = "SSL for the blog"

+++

I've decided that it would be good to have this blog running on `https://` instead of plain `http://`. A friend of mine, [Vladimir Starkov](https://twitter.com/iamstarkov), suggested to look into [Cloudfare](https://www.cloudflare.com).

Setting up `https://` turned out to be a rather simple task. I've followed [this](https://www.benburwell.com/posts/configuring-cloudflare-universal-ssl/) and [this](https://support.cloudflare.com/hc/en-us/articles/200171566-How-do-I-change-my-domain-nameservers-GoDaddy-) guides to get the job done. It seems that some of the configuration options at least on GoDaddy are sort of hidden away. At least I didn't see at first where to change nameservers. Well, I guess that's just because I'm really not familiar with their admin panel. Cloudfare's UI is rather nice compared to GoDaddy though. I've found all I needed to find there quite fast.
