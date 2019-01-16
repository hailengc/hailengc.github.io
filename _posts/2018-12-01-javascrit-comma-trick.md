---
layout: post
comments: true
title: "Zero, Comma and parentheses - A Javascript Trick"
categories: [coding, javascript]
tag: [codeing, javascript]
description: "A javascript trick use zero, comma and parenthese. About Javascript reference type."
---

When I was digging into the source code of [React Antd](https://ant.design/docs/react/introduce), this line took my notice. Actually, later I found this trick is heavily used through the whole project.

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

The line with comment may seems interesting, it can be simplified as:

```javascript
(0, obj.method)(targetObject, param1, ...)
```

Sure it's a method call, but why need the parenthesis, zero and the comma 🤔

It's all about [Reference Type](https://javascript.info/object-methods#internals-reference-type) - read this carefully. Reference type is an internal merchanism that js runtime leverages when dealing with function call. I will illustrate with examples (thanks for the beautiful codesandbox):

<iframe src="https://codesandbox.io/embed/9805n7n1q4?autoresize=1&expanddevtools=1&fontsize=13&hidenavigation=1&module=%2Fsrc%2Findex.js&view=editor" style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;" sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>

So, things are clear here: the zero is nothing but a "placeholder", it exists just to ensure the parenthesis will grab off the reference type of object method. The whole trick is a claim says:

> "Though I might look like a method bound to an object, but I don't use 'this' at all, please treat me like a util method."

You could write `obj.method.call(...)` instead, but use comma and parenthesis is more semantically clear. Last, hope [tc39](https://github.com/tc39) will include my propoal: `&obj.method(target, parm1...)` 😂
