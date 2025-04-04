---
title: "Create a blog using Hugo and Github Pages"
description: "Learn how to setup and host your own website using a static html generator - Hugo."
tags: ["hugo", "web", "linux"]
date: 2021-04-05T14:13:49+09:30
featured_image: "/images/hugo-logo.png"
---
Update: Added section on adding Github Action for Hugo build

This is a guide on how you can create a blog or website from scratch using Hugo and Github. This is also my first post on this blog, which has been created using the steps mentioned in this article using Hugo and Github Pages.

It is quite easy to create a blog using Wordpress or some other CMS but the simplicity of Hugo is what drove me to it. Hugo is a static html generator. This means, you write posts using Markdown and templates using any text editor. Hugo then processes these files and generates a bunch of html and css. 

This has the benefit of making it extremely easy to deploy to just about any cloud service or provider. This is where Github Pages comes in. Github Pages is free and if you have a Github account, you are already well on your way.

I used a Macbook Air M1 for creating this blog and the steps outlined in this article should be more or less the same whether you use Windows / MacOS or Linux. Lets get our elbows greasy.

### Create Repos
We will begin by creating 2 repositories in our Github account -
1. Base repo 'blog' for hosting our posts / source code / files created by Hugo
2. Blog hosting repo 'username.github.io' for hosting the actual blog, which consists of generated html and css

Create the base repo on Github by clicking the '+' button on your [Github page](https://www.github.com). I've decided to call my base repo 'blog'. Clone this repository to your machine (even if it doesn't contain any files at the moment)
```bash
git clone git@github.com:username/blog.git
cd blog/
```

From the Github website, click the '+' button again to create a new repository called 'username.github.io'. Replace username with your Github user name.

Add this second repo (which we will use for hosting our blog) as a submodule of our base repo -
```bash
git submodule add git@github.com:username/username.github.io.git public
```
The above command will add the hosting repo as a submodule into the 'public' folder.

### Install Hugo
Download the Hugo binaries from the [Hugo Releases](https://github.com/gohugoio/hugo/releases) page. As I am using MacOS, I used the command -
```bash
brew install hugo
```
For detailed installation instructions, visit https://gohugo.io/getting-started/installing/
Verify Hugo installed correctly by 
```bash
hugo version
```

### Create a new site
```bash
hugo create new site myblogname
```
The above command will create a basic website with enough boilerplate to let us get started.

### Add a theme
Visit https://themes.gohugo.io/ for a collection of available Hugo themes. Once you have decided which theme you wish to use for your website / blog, 
```bash
cd myblogname
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
```
Your folder structure should now look like
**~/blog/myblogname/config.toml** | Configuration File
**~/blog/myblogname/themes/ananke/** | Theme folder
**~/blog/myblogname/public** | Second hosting repo

### Update config.toml 
```toml
theme = "ananke"
title = "Karan Kadam"
baseURL = "https://username.github.io"
theme = "ananke"
themesDir = "./themes"
```

### Create your first post
```bash
hugo create new posts/my-first-post.md
``` 

### Build your blog
```bash
hugo -t "ananke"
```
This will generate all the html and styling required for your blog and publish them under the "public" folder. The -t flag tells hugo to use your selected theme.

### Local test
```bash
hugo server --bind 0.0.0.0
```
This will launch the built in webserver and your blog will be available locally. Goto a webbrowser and access http://localhost:1313 and you should see your blog, styled according to the theme you chose and containing your first sample post.

### Deploy your website to Github Pages
To be able to share your website with the rest of the world, you will need to host it somewhere. We chose the repository at 'https://username.github.io'. Check in all your changes and push them to the username.github.io -
```bash
cd ~/blog/myblogname/public
git status
git add -A
git commit -m "initial test commit" # commit contents of ~/blog/myblogname/public
git push
cd ../..
git add -A
git commit -m "initial test commit" # commit contents of ~/blog/
```
If it all went well, accessing http://username.github.io should take you to your freshly baked blog. Here on, all you need to do is,
```bash
hugo create new posts/postname.md
hugo -t "ananke"
```

### Markdown resources
It is a good idea to read up and familiarise yourself with Markdown Syntax. It is quite compact and can be learned in a single sitting. I would recommend starting with [The Markdown Cheatsheet](https://guides.github.com/pdfs/markdown-cheatsheet-online.pdf) to quickly understand how to write posts in any editor using the Markdown syntax. Keep writing! :metal:

### Bonus - Automate the Hugo build
To automate your flow even further, you can create a Github Action, which will automatically run Hugo for you and publish to your username.github.io repository for you. This will save you any manual work of building your blog each time and pushing back to your public repo.
The added benefit of this approach is you no longer need access to the cli. You can log into your Github base repo from your phone, add Mardown posts from the WebUI and your Github Actions pipeline will take care of the rest.
Begin by adding a GitHub action file to your base repo -
```bash
vim ./github/workflows/main.yml
```
Here is a what my main.yml file looks like -
```yaml
name: CI
on: push
jobs:
  deploy:
    runs-on: ubuntu-18.04
    steps:
      - name: Git checkout
        uses: actions/checkout@v2

      - name: Update theme
        run: git submodule update --init --recursive

      - name: Setup hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: "0.82.0"

      - name: Build
        run: cd username && hugo -t ananke && pwd && ls -a ./public

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          deploy_key: ${{ secrets.ACTIONS_DEPLOY_KEY }}
          external_repository: username/username.github.io
          publish_dir: ./intothevoid/public
          user_name: username
          user_email: username@email.com
          publish_branch: main
```
Replace values with your own where needed.

A quick explanation of the above Github Action is given below -
1. Checkout - checkout the base repo
2. Update - Update your theme which was added as a submodule
3. Setup Hugo - Call an external action which basically installs Hugo
4. Build - Build the website by calling **hugo -t ananke**
5. Deploy - Publish the built files to your external public repo username.github.io

**Note:** There is a bit of setup involved where you need to generate your token (indicated by deploy key above). I would highly recommend visiting https://github.com/peaceiris/actions-gh-pages for details of the external action and how to setup your token.

This should get you at a stage where you can simply push a post written in Markdown format to your base repo and the rest should happen automatically. Post --> Commit Base Repo --> Github Action --> Build Blog --> Publish Static HTML.
