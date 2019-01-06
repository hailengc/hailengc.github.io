---
layout: post
comments: true
title: "BinaryHeap, a simple Ruby gem"
categories: [coding, ruby]
tag: [ruby]
description: "standard Ruby binary heap implementation."
---

I've been using ruby for quite a long time. Ruby is really a pretty language which you would like to use it anywhere whenever you can.

When I was solving a problem that requires a binary heap data structure, I couldn't find a simple gem to help me so I wrote [this gem](https://rubygems.org/gems/binaryheap).

Binary heap is a common structure in computer science. It is very useful in sovling kth problem. The implementation is easy and straightforward: just use the Array data structure with some extra work on bubbling element up and down.

One thing needs to be considered is multi-threading safe. When Ruby program is running on a CRuby environment, eveything is OK - "thanks" for the GIL. Things are different when you run it with JRuby, there is no GIL in JVM so multi-threading in JRuby has way better performance than CRuby - however its our duty to keep things right. Howto solve this? Easy enough, use a mutex:

```ruby
# Insert an element into the heap (heap will be adjusted automatically)
# @param element [Object] the element to be inserted into the heap
# @return [BinaryHeap] the binary heap instance itself
def insert(element)
    @mutex.synchronize do
        @ary.push(element)
        adjust(:bottom_up)
    end
    self
end

```

Check the [repo](https://github.com/hailengc/binaryheap) if you are interested in my work.
