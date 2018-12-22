---
layout: post
comments: true
title: "jekyll-miniaudio, a Jekyll audio plugin"
categories: [coding, ruby]
tag: [Jekyll plugin, html5, audio]
description: "Jekyll plugin, html5 audio"
publised: false
---

I made a minimal audio plugin for Jekyll: jekyll-miniauido. Checkout [demo](https://hailengc.github.io/jekyll-miniaudio) and the
[github repository](https://github.com/hailengc/jekyll-miniaudio).

<iframe src="https://hailengc.github.io/jekyll-miniaudio/" width='100%' height='180px'></iframe>

When I want a plug-and-play audio plugin for Jekyll, I didn't find a good one so I ended up with this simple plugin.

The implementation is quite simple. What I want is a **tag** that will generate some simple HTML + js + css content, the usage should be like this:

`miniaudio '/assets/music/demo.mp3'`

So, with this goal in mind, [Jekyll doc on tag](https://jekyllrb.com/docs/plugins/tags/) is there to help me. All I need to do is to implement a render method and register this tag.

First we need to crate a gem, the file structure looks like this:
![file structure](/assets/images/jekyll-miniaudio-structure.jpg)

The h5audio folder holds all the static files (html, js, css) I wish to render, and the miniaudio.rb is the core functional part, here's the code in it:

```ruby
require 'jekyll'
require 'fileutils'
require_relative 'miniaudio/version'

class MiniAudio < Liquid::Tag
  def initialize(tag_name, audio_url, options)
    super
    @audio_url = audio_url
  end

  def render(_context)
    ma_path = File.join File.expand_path(__dir__), 'miniaudio'
    template = File.read(File.join(ma_path, 'h5audio', 'template.html'))
    Liquid::Template.parse(template).render(
      'audioSrc' => @audio_url,
      'title' => File.basename(@audio_url, '.*'),
      'assets_path' => "/assets/miniaudio-#{Jekyll::Miniaudio::VERSION}",
      'id' => Random.rand.to_s
    )
  end
end

Jekyll::Hooks.register :site, :after_init do |site|
  destination_dir = site.config['destination']
  dest_assets_path = File.join destination_dir, 'assets', "miniaudio-#{Jekyll::Miniaudio::VERSION}"
  src_asset_path = File.join File.expand_path(__dir__), 'miniaudio', 'h5audio'
  unless Dir.exist?(dest_assets_path)
    FileUtils.cp_r(src_asset_path, dest_assets_path)
  end

  # make sure directory _site/assets/miniaudio-*.*.* will be kept by jekyll
  keep_files = site.config['keep_files']
  asset_path = "assets/miniaudio-#{Jekyll::Miniaudio::VERSION}"
  keep_files.push(asset_path) unless keep_files.include?(asset_path)
end

Liquid::Template.register_tag('miniaudio', MiniAudio)

```

Here's some points:

- In render method, use Liquid::Template to render the template html. Since we have `<link>` and `<srcipt>` in html, it will automatically load js and css files.
- Call `Jekyll::Hooks.register` to register our plugin, two things here:

  1. Ensure the h5audio folder is copied into Jekyll's destination folder (generally it's the `_site` folder )
  2. Ensure directory `_site/assets/miniaudio-*.*.*` will be kept by Jekyll when building, we need to dynamically set the `keep_files` config. See [here](https://jekyllrb.com/docs/configuration/options/)

I didn't mention about the HTML/js/css implementation cause it's straightforward, you can checkout the [github repository](https://github.com/hailengc/jekyll-miniaudio) for the detail.
