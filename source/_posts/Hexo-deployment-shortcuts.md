---
title: Shortcuts for your Hexo deployment
date: 2021-12-15 19:19:51
tags:
- Hexo
- Git
- GitHub Pages
- Blogging
---

Good morning!

In [my last post](/2021/12/13/Deploying-your-Hexo-blog-on-GitHub-Pages/), I showed you how to deploy your Hexo blog on GitHub pages. If you're already `git`-savvy, then deploying your blog this way is a treat. But we can make the process even simpler, with a few shortcuts.

## Scaffolds

When you run `hexo new <title>`, Hexo creates a new blog post based on a ["scaffold" file](https://hexo.io/docs/writing#Scaffolds), stored in the `scaffolds` directory of your Hexo project. If you haven't changed any of the defaults in your Hexo configuration file (`_config.yml`), it specifically uses `scaffolds/post.md`.

By default, this file is pretty small, as we can see by using `cat` to print its contents:

```
$ cat scaffolds/post.md
---
title: {{ title }}
date: {{ date }}
tags:
---
```

But even this helps us out by automatically filling in the title of the post, and the date that it was created! Check out the contents of the new post:

{% code mark:6-7 %}
$ hexo new "My post"
INFO  Validating config
INFO  Created: ~/repos/hexo/murchu27.github.io/source/_posts/My-post.md
$ cat source/_posts/My-post.md
---
title: My post
date: 2021-12-15 19:37:41
tags:
---
{% endcode %}

Of course, you can edit this scaffold file however you like!

For instance, in both this post and the last, I wished you all a "Good morning"! Now I quite like the mindful act of typing this out manually, but if liked, I could make a change to my scaffold file, so that every new post started with "Good morning"!

Take a look at how a change like that affects new posts:

```
$ cat scaffolds/post.md
---
title: {{ title }}
date: {{ date }}
tags:
---

Good morning!

```

{% code mark:11 %}
$ hexo new "My robotic post"
INFO  Validating config
INFO  Created: ~/repos/hexo/murchu27.github.io/source/_posts/My-robotic-post.md
$ cat source/_posts/My-robotic-post.md
---
title: My robotic post
date: 2021-12-15 20:01:51
tags:
---

Good morning!

{% endcode %}

If you're regularly posting, say, a daily update on the stats of the top Formula One racers, then you could put a list of those racers in your scaffold file so that you don't have to type them out every time!

## Simplifying your commits

Now, when it comes to using `git` for code, I'm constantly bugging my fellow devs to write descriptive commit messages, so that the rest of us can easily follow what they've changed.

But with blog posts, that's not so important. For the most part, you're (probably) writing new posts much more than updating old ones, and there's no logic that needs explaining. If you're using GitHub, that has plenty of features to check when and how files were updated, without the need for reading your commit messages.

That in mind, we can make our lives a bit easier by making a one-line command for updating our site.

There are a couple of ways you can do this, I've decided to use a custom `npm` script. In your Hexo project directory, open up `package.json`, and add the highlighted line in the "scripts" section:

{% code mark:8 %}
{
  "name": "hexo-site",
  "version": "0.0.0",
  "private": true,
  "scripts": {
    "build": "hexo generate",
    "clean": "hexo clean",
    "update": "git add source/* && git commit -m \"Site updated: `date`\" && git push",
    "deploy": "hexo deploy",
    "server": "hexo server"
  },
  ...
}
{% endcode %}

Now, we can run `npm run update`, and this will execute <code>git add source/* && git commit -m "Site updated: \`date\`" && git push</code> for us! This commits any changes to our blog posts, writes a default message, and pushes to GitHub.

## That's all for now

With scaffolds and simpler commits, you can reduce the time spent on the repetitive stuff, so you can focus on the stuff that matters - the writing!
