---
title: deploying and maintaining something like this easily
date: 2023-04-26 21:42:00 +/-TTTT
categories: [tutorials]
tags: [simple]     
---

This guide is for my friend to make a quick personal site. There are lots of great tutorials out there; however, I didn't see anything concise enough that included all steps. We will:
- Use Jekyll as the framework for heavy lifting
- Deploy with GitHub pages
- Not spend money or time, go play with your dog

1. Create a new GitHub repo with your Jekyll scaffolding
	- [Click](https://github.com/cotes2020/chirpy-starter/generate) to use the basic theme I use. Upgrading is [easy](https://github.com/cotes2020/jekyll-theme-chirpy/wiki/Upgrade-Guide#upgrade-the-fork)
2. If you want to pull locally
	1. Install [Jekyll](https://jekyllrb.com/docs/installation/) and [Git](https://git-scm.com/)
	2. Start the server with `bundle exec jekyll s`
3. Deploy repo with GitHub actions
	1. Ensure it's public
	2. Click *Settings* > *Pages* > *Source* > *GitHub actions*

New commits will kick off the GH actions deployment. That's it!

---
#### More

If you want to use a domain name
- Configure your DNS records as follows:

|         Host name            |  Type     |   TTL  |                                   Data                                           |   |
|:----------------------------:|:---------:|:------:|:--------------------------------------------------------------------------------:|---|
| `<YOURWEBSITE>.com`    | A         | 1 hour |  185.199.109.153<br />185.199.108.153<br />185.199.110.153<br />185.199.111.153  |   |
| `www.<YOURWEBSITE>.com` | CNAME     | 1 hour |  `<YOUR GITHUB USERNAME>.github.io.`                                                          |   |

- In your GH repo, click *Settings* > *Pages* and add your site to the *Custom Domain* field in with the syntax `<YOURWEBSITE>.com`

---

[Ghost](https://ghost.org/) is a cool alternative to this method. Apparently, you can publish [Obsidian](https://obsidian.md/) to it; however, Ghost costs $$$. 😞

References
- [Chirpy wiki](https://github.com/cotes2020/jekyll-theme-chirpy/wiki)

> If you know of a more simple way to do this, please let us know in the comments!
{: .prompt-info }