---
title: Deploying your Hexo blog on GitHub Pages
date: 2021-12-13 22:19:42
tags:
- Hexo
- Git
- GitHub Pages
- Blogging
---

Good morning!

Last night, I deployed my first blog using Hexo. It involved the following steps:

1. Install prerequisites
2. Install Hexo
3. Create a Hexo project       
4. Create a blog post
5. Deploy using GitHub Pages

For steps 1-3, I'd recommend using Hexo's own installation instructions, as those are clear and easy to follow. I'm mostly writing this post to give additional help for step 4, and alternate instructions for step 5!

## Install prerequisites & Install Hexo
Use [Hexo's documentation](https://hexo.io/docs/#Installation)!

## Create a Hexo project
Use [Hexo's documentation](https://hexo.io/docs/setup)!

## Create a blog post
[Hexo's documentation](https://hexo.io/docs/writing) is also good here, but fails to mention how to view your posts in a browser!

Firstly, create a new blog post with:

```
$ hexo new "My blog post title"
```

This will create your post at `source/_posts/My-blog-post-title.md`, which you can open up in your favourite editor, and fill with delightful puns or other such important content.

(Heads up: you'll be writing in [Markdown](https://www.markdownguide.org/))

Then, to see your post for reals, you can launch a [temporary server](https://hexo.io/docs/server) by running:

```
$ hexo server
```

And then you can open up `http://localhost:4000` in your browser. That is, assuming you're running Hexo on the same computer that your browser is on. If you're running Hexo on another machine (e.g., a Raspberry Pi), then go to `http://{ip-address-of-pi}:4000` instead.

Behold the glory of your blog post!

## Deploy using GitHub Pages
As I mentioned, using `hexo server` to see your blog posts is a temporary measure. `hexo server` is constantly monitoring your filesystem for changes (which you don't want while you're working on a post), and is generally overkill seeing as a blog is just a static website.

Instead, Hexo offers many options for converting your blog posts to static HTML, and automatically deploying them to whatever location you like. A popular choice is [GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages); a static site hosting service that is completely free for anyone with a GitHub account!

As always, [GitHub's documentation is excellent](https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site), but I actually wouldn't recommend using it here. They presume that you are using [Jekyll](https://jekyllrb.com/) for creating your blog posts, so some of the steps cause issue later.

[Hexo's documentation](https://hexo.io/docs/github-pages.html) is the best thing to follow for this, but stop before creating the `.github/workflows/pages.yml` file, and come back here.

If you're in a hurry, the change is simple.

First, check your version of Node.js with:

```
$ node --version
v16.13.1
```

Make a note of the major version (v16.x in this case).

Then, create the `.github/workflows/pages.yml`, exactly as described in the [Hexo docs](https://hexo.io/docs/github-pages.html), but find the Node.js section, which looks like:

```
- name: Use Node.js 12.x
  uses: actions/setup-node@v1
  with:
    node-version: '12.x'
```

And change it to the below (using whatever major version of Node.js you found in the last step).

```
- name: Use Node.js 16.x
  uses: actions/setup-node@v1
  with:
    node-version: '16.x'
```

You can continue with the Hexo docs from here!

If you've finished those - congratulations on your first public blog post!

### For the curious

When I made this post, the Hexo docs were still recommending using Node.js 12.x in this workflow. This may cause issue if you are using a newer version of Node.js on the machine where you write your Hexo posts.

This is because Node.js v15.x and beyond use a newer version of the `package-lock.json` file that may get generated if you have installed extra modules with `npm`. Check out npm's documentation on the [`package-lock.json`](https://docs.npmjs.com/cli/v7/configuring-npm/package-lock-json) file, specifically the section on the [`lockfileVersion`](https://docs.npmjs.com/cli/v7/configuring-npm/package-lock-json#lockfileversion)

If you push a `package-lock.json` with `lockfileVersion: 2` to your GitHub repository, and your GitHub workflow tries to read this with an older version of Node.js like v12.x, your workflow might fail...

Making the change above should solve any `package-lock.json` related issues you may have! Don't just take my word for it though - check out a [failed run of the workflow](https://github.com/murchu27/murchu27.github.io/runs/4499900719?check_suite_focus=true) for this website. As soon as I updated the Node.js version in `.github/workflows/pages.yml`, [the workflow ran successfully](https://github.com/murchu27/murchu27.github.io/runs/4499912021?check_suite_focus=true).

Thanks for following along, happy blogging!
