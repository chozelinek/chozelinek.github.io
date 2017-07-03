## José Manuel Martínez Martínez's website

This website uses `Jekyll` and the `minimal-mistakes-jekyll` theme. This is not a supported theme by GitHub Pages.

I detail bellow all the steps I followed to set up this website.

I used as reference <https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/> and <http://tscholak.github.io/update/guide/jekyll/2015/03/06/jekyll-and-github-pages.html>

## Installing Jekyll

Check whether you have Ruby 2.1.0 or higher installed:

```shell
ruby --version
```

Install Bundler (kind of ruby package manager equivalent to pip):

```shell
gem install bundler
```

Create the local git repository:

```shell
mkdir -p ~/Projects/chozelinek.github.io
```

Change to this directory:

```shell
cd ~/Projects/chozelinek.github.io
```

Create a file called `Gemfile` and type the following text:

```yaml
source 'https://rubygems.org'
gem 'github-pages' # github-pages
gem 'minimal-mistakes-jekyll' # theme
```

Install with the following command to avoid errors in Mac OS Sierra:

```shell
brew unlink xz; bundle install; brew link xz
```

## Configuring the site and setting up the structure

### General site configuration

<https://mmistakes.github.io/minimal-mistakes/docs/configuration/>

Content for `_config.yml`:

```yaml
theme: minimal-mistakes-jekyll
locale: "en-US"
title: "José Manuel Martínez Martínez"
name: "José Manuel Martínez Martínez"
description: "(computational) linguist & translator"

author:
  name: "José Manuel Martínez Martínez"
  avatar: "https://avatars0.githubusercontent.com/u/2077497?v=3&s=460"
  bio: "Born in Valencia, Spain. Currently in Saarbrücken, Germany. Passionate about languages, coding, climbing, performing arts, and travelling."
  email: # optional
  uri: "https://chozelinek.github.io"
  github: chozelinek
  twitter: chozelinek
  linkedin: # optional

include:
    - "_pages"
    - ".nojekyll"

exclude:
    - LICENSE
    - README.md
    - Gemfile
    - Gemfile.lock

defaults:
  # _pages
  - scope:
      path: ""
      type: pages
    values:
      layout: single
      author_profile: true
```

### Navigation

<https://mmistakes.github.io/minimal-mistakes/docs/navigation/>

Create file `_data/navigation.yml`:

```shell
mkdir -p _data/
touch _data/navigation.yml
```

Add the following content:

```yaml
# main links
main:
  - title: "CV"
    url: /cv/
  - title: "Publications"
    url: /publications/
  - title: "Blog"
    url: /blog/
```

### Pages

Follow the instructions here: <https://mmistakes.github.io/minimal-mistakes/docs/pages/>

Create folder:

```shell
mkdir -p _pages
```

#### cv

Create file:

```shell
touch _pages/cv.md
```

Add the following content:

```
---
title: My Curriculum Vitae
permalink: /cv/
---
```

#### publications

Create file:

```shell
touch _pages/publications.md
```

Add the following content:

```
---
title: My Publications
permalink: /publications/
---
```

### Setting the home page

Create a file called `index.md` and be sure it contains the following:

```markdown
---
layout: single
author_profile: true
title: Me in a Nutshell
---
```

## Setting up the git infrastructure

Initialize local repository:

```shell
git init
```

Create a file called `.gitignore` and add the following content:

```shell
_site/*
.sass-cache/*
```

Create a file called `.nojekyll` to bypass automatic build.

```shell
touch .nojekyll
```

Commit the site we just generated:

```shell
git add .
git commit -m "generate site with Jekyll"
```

Create the remote:

1. go to <https://github.com>
1. log in
1. click `+` sign on top left corner
1. click `new repository`
1. Repository name: `chozelinek.github.io`
1. click `Create repository`

Add the remote to the local:

```shell
git remote add origin git@github.com:chozelinek/chozelinek.github.io.git
```

GitHub Pages renders the content of `master` branch for user pages.

We need to create a branch called `source` and a branch called `master` where we will push the files resulting from building the site.


Create the `source` branch:

```shell
git checkout -b source master
```

Push the new local branch to the remote:

```shell
git push -u origin source
```

Delete the master branch:

```shell
git branch -D master
git push origin :master # maybe not necessary the first time
```

Recreate master but empty:

```shell
git checkout --orphan master
git reset .
rm -r *
rm .gitignore
rm .nojekyll
```

```
git commit --allow-empty -m "clean slate"
git push -u origin master
```

Change to branch `source`:

```
git checkout source
```

Checking out `master` into `_site`:

```
git clone git@github.com:chozelinek/chozelinek.github.io.git -b master _site
```

Build the site:

```shell
bundle exec jekyll build
```

Change directory:

```shell
cd _site
```

Commit changes:

```
git add .
git commit -m "first build"
```

Push changes:

```shell
git push
```

## Adding content to the website: the typical workflow

Go to the local repo:

```shell
cd ~/Projects/chozelinek.github.io
```

Switch to the source branch:

```shell
git checkout source
```

Get changes (unlikely):

```shell
git pull
```

Modify/add whatever locally

Then, add all, commit to local, and push to remote:

```shell
git add -A && git commit -m "[message]" && git push
```

Now build:

```shell
bundle exec jekyll build
```

Change directory to `_site`:

```shell
cd _site
```

Automagically we switch to branch `master`.

We just add all, commit locally, and push the static generated files to the master branch:

```shell
git add -A && git commit -m "[another_message_referencing_the_first_message]" && git push
```

## Bibliographies

Use jekyll-scholar <https://github.com/inukshuk/jekyll-scholar>

Add to `Gemfile`:

```
gem 'jekyll-scholar'
```

Update the bundle:

```shell
brew unlink xz; bundle update; brew link xz
```

Add the following content to `_config.yml`:

```yaml
gems: ['jekyll/scholar']

scholar:
    style: apa
    source: ./_bibliography
    bibliography: MyPublications.bib
    sort_by: [year, month]
    order: descending
```

Create folder called `_bibliography`:

```shell
mkdir -p _bibliography
```

Copy your most recent bibliography version into this folder:

```shell
cp ~/Dropbox/BIBLIOGRAPHY/BibTeX/MyPublications.bib ./_bibliography/
```

Create the page `_pages/publications.md`.

Include the link to a downlodable PDF version in `assets/publications.pdf`.


## Creating a blog

### Configuration

First one needs to modify the `_config.yml` file to enable a default template for blog posts. Add under defaults the following lines:

```yaml
# _posts
- scope:
    path: ""
    type: posts
  values:
    layout: single
    author_profile: true
    read_time: true
  #   comments: true
    share: true
    related: true
```

### Create a blog index

Next create a blog index. A landing page where readers will see the latest posts published in the blog.

Add the following content to `_config.yml`:

```yaml
gems:
    - jekyll/paginate
```

```yaml
paginate: 5
paginate_path: /blog/page:num
```

Create the file `blog/index.html`.

Add the following content:

```
---
permalink: /blog/
title: Blog
tagline: All posts
layout: home
---
```

Add to `_data/navigation.yml` the lines:

```yaml
main:
  - title: "Blog"
    url: /blog/
```

> Tip: install `jekyll-compose` to ease the drafting and publishing workflow. <https://github.com/jekyll/jekyll-compose>

### Working with posts

To create a post just:

```shell
bundle exec jekyll post "My New Post"
```

### Organizing blog content with tags and categories

<https://codinfox.github.io/dev/2015/03/06/use-tags-and-categories-in-your-jekyll-based-github-pages/>

Use the basic `liquid` based `GitHub pages` compatible option.

Create the file [`categories/index.html`](https://github.com/mmistakes/minimal-mistakes/blob/master/docs/_pages/category-archive.html).

Create the file [`tags/index.html`](https://github.com/mmistakes/minimal-mistakes/blob/master/docs/_pages/tag-archive.html)

Add to `_config.yml`:

```yaml
category_archive:
    type: liquid
    path: /categories/
tag_archive:
    type: liquid
    path: /tags/
```

Finally, in order to show author information on the left add the following to `_config.yml`:

```yaml
defaults:
  # _archives
  - scope:
      path: ""
      type: liquid
    values:
      author_profile: true
```

### Working with drafts

Create the draft:

```shell
bundle exec jekyll draft "My new draft"
```

Publish the draft:

```
bundle exec jekyll publish _drafts/my-new-draft.md
```

## Frequently used Jekyll commands

Update the bundle:

```shell
brew unlink xz; bundle update; brew link xz
```

Build the website:

```shell
bundle exec jekyll build
```

Start the local server:

```shell
bundle exec jekyll serve
```

With drafts:

```shell
bundle exec jekyll build --drafts
bundle exec jekyll serve --drafts
```
