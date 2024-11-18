---
title  : Jekyll
layout : default
parent : Framework
---

# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Install Jekyll and use default config

1. Install _Ruby_ and _Bundler_ will be installed too.
    ```
    apt install ruby
    ```
2. Install _Jekyll_ using _Bundler_.
    ```
    bundle init
    bundle add jekyll 
    ```
3. Initialize _Jekyll_.
    ```
    bundle exec jekyll new <folder> --force
    bundle add webrick
    ```
4. Start _Jekyll_ webserver and start tweaking.
    ```
    bundle exec jekyll serve
    ```

## Theme: just-the-docs

Currently I'm using this theme and hosted it on Github Pages.

### Usage

1. Open up `_config.yml` and edit to your liking.
2. Go to [Just the Docs](https://pmarsceill.github.io/just-the-docs/) for further documentation.
