---
layout: post
comments: true
title: "Bubble animation with Paper.js"
date: 2018-11-18 11:21:30 +0800
categories: [coding, visualization]
tag: [Bubble animation, paper.js, visualization, react]
description: "A simple 2D bubble animation "
---

![bubble-animation](/assets/images/bubble-animation.gif)

During this weekend, I made a simple bubble animation using React and [Paper.js](http://paperjs.org/). Check the [live demo](https://hailengc.github.io/bubble-fun/index.html#/).

I'm been working on a web project using React and Paper for quite a long time. The ease of using Paper.js really impressed me. Also, those great [showcases](http://paperjs.org/examples/) are also inspiring me on new ideas.

My first idea is bubble animation. Bubble is basically a circle but not static, it is **dynamic**. That means when a bubble is floating in the air, its circle boundary is vibrating (caused by the air pressure or air flow). Well... sounds complicated - anyway I don't want make a physical simulation on this, dude!

Since visualization is the goal. I can just use rotating and scaling to achieve the goal. Here's the points:

- change the scaling on each frame with some random
- rotate the cicle on each frame with some random
- move the bubble using a math `sin` function so it can float up in a sin-like curve

With these 3 points, basically a dynamic bubble is done. The rest part of the implmentation is the management of those bubbles - This is where paper.js realy helps. Paper.js has clear concepts like Item, Path, View, Layer which make object management easy enough.

```javaScript
animate = event => {
  this.paintLayer.children.forEach(b => {
    if (Bubble.isBubble(b)) {
      b.animate(event);
      if (!this.paperView.bounds.intersects(b.bounds)) {
        b.remove();
      }
    }
  });
};
```

`paintLayer.children` fetches all child object and make it animate. If bubble float outof our view (`paperView.bounds.intersects` ), we just remove it from our paintLayer.

If you are interested in my work, check the [repo](https://github.com/hailengc/bubble-fun).
