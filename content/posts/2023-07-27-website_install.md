---
title: Installation amd setup of Jekyll
date: 2023-07-27
tags:
  - jekyll
  - setup
categories:
  - web
author_profile: true
draft: false
comments: false
slug: installation-amd-setup-jekyll
---

This little guide assumes the use of Mac. It was created on a MacOS 15 for deployment on the local machine and versioning in GitHub.

## Prerequisites

The following software is needed for the installation:

- GitHub account
- Ruby
- Bundler
- Jekyll

Additionally, it can be use ful to have a template for Jekyll. In this case I am using the [Minimal Mistakes theme](https://mmistakes.github.io/minimal-mistakes/).

## Installation

### Ruby and Bundler

In my case a Ruby version and a Bundler executable are already installed in the system

```zsh
which ruby
/usr/bin/ruby
which bundler
/usr/bin/bundler
```

However, the system installation may be outdated, and in general is advisable not to modify it to not risk having parts of the operating system stop working suddenly. In my case the system-wide Ruby turned out to be `ruby 2.6.10p210`, quite old.
I installed a new version of the two softwares using [Homebrew](https://brew.sh).

```zsh
brew install ruby
```

To check the new version of Ruby is installed

```zsh
/usr/local/opt/ruby/bin/ruby --version
ruby 3.2.2 (2023-03-30 revision e51014f9c0) [x86_64-darwin21]
```

Now that I have a new running version of Ruby, we can use it to install Jekyll with

```zsh
/usr/local/opt/ruby/bin/gem install jekyll
```

### Minimal Mistakes

We install the theme by cloning the Minimal Mistakes repository

```zsh
mkdir bign86.github.io
cd bign86.github.io
git clone https://github.com/mmistakes/minimal-mistakes.git
```

and deleting all the files and folders that are not needed to run the website

```zsh
rm -rf CHANGELOG.md README.md .git/ .editorconfig .gitattributes .github docs/ test/ minimal-mistakes-jekyll.gemspec screenshot.png screenshot-layouts.png
```

### Serving the website

Now, we can simply install the required gem files into Ruby using Bundler, and serve the website

```zsh
bundle install
# some time is needed for completion...
bundle exec jekyll serve
```

The website should now be reachable at [localhost:4000](http://127.0.0.1:4000).

## Storing the file on GitHub

I now want to store the content of the website on GitHub. For this I created a repository with a name in the form `bign86/bign86.github.io` in case I want to also publish the website externally on GitHub Pages in the future.
First, I initialize a new git repo in the folder where we are working

```zsh
cd ..
git init bign86.github.io
```

and I connect the new repo to the correct remote and check that it has been set correctly

```zsh
git remote add origin https://github.com/bign86/bign86.github.io.git
git remote -v
origin https://github.com/bign86/bign86.github.io.git (fetch)
origin https://github.com/bign86/bign86.github.io.git (push)
```

Last, I commit everything and push to my new repository

```zsh
git add *
git commit -m "first commit"
git push --set-upstream origin master
```

## Bash and aliases

It is possible to add the path of the newly installed Ruby version in Zsh with

```zsh
echo 'export PATH="/usr/local/opt/ruby/bin:$PATH"' >> ~/.zshrc
```

The same with Bundler.
