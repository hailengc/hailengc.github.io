---
layout: post
comments: true
title: "Comma and parentheses - a Javascript Trick"
categories: [coding, javascript]
tag: [codeing, javascript]
description: "A javascript trick use comma and parenthese"
published: false
---

See below code and the comment:

```javascript
exports.default = function(obj, key, value) {
  if (key in obj) {
    //  what the hell is this line doing?
    (0, _defineProperty2.default)(obj, key, {
      value: value,
      enumerable: true,
      configurable: true,
      writable: true
    });
  } else {
    obj[key] = value;
  }

  return obj;
};
```
