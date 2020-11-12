---
layout: post
title: Blink HTML - how to make blink HTML these days 😉
comments: False
---
The Blink HTML was first visual effect that was available in HTML. Your HTML page could show text, use different colors and blink.
How that happened and how to make blink HTML nowadays (or blink HTML5)

<!--more-->

Alright, most of the people I know thing that blink HTML is annoying. It is a part of history now, modern HTML pages use nice CSS3 and JavaScript-based effects to make your HTML page blink and do all other kind of stuff (WebGL, WebXR, 3d, anything).

Lou Montulli is often credited as a creator of blink HTML tag, and he has an interesting story <a href="http://www.montulli.org/theoriginofthe%3Cblink%3Etag" target="_blank">about the origins on blink HTML tag</a>.

My favorite quote there is: 

>and suddenly everything was blinking.    "Look here", "buy this", "check this out", all blinking.  

Reminds me <a href="https://www.youtube.com/watch?v=YlGklt4BSQ8" target="_blank">Futurama's laugh at VR ads</a> <span class="blink">😉</span>


### How to make blink HTML work nowadays? 

Enough history!
Good thing about any of the ways below is they support any content, not just text

<a href="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/blink" target="_blank">MDN recommends a blink HTML polyfill using CSS</a> which uses keyframes and will make blink HTML tag blink again.

```
blink {
  animation: 2s linear infinite condemned_blink_effect;
}

@keyframes condemned_blink_effect {
  0% { visibility: hidden; }
  50% { visibility: hidden; }
  100% { visibility: visible; }
}
```

Wikipedia's way to make blink HTML work again:

```
blink, .blink {
  animation: blink 1s step-end infinite;
}

@keyframes blink {
  67% { opacity: 0 }
}
```

JavaScript way to make a blinking HTML element (not only text)

```
const blink = document.getElementById('blink');
setInterval(() => {
    blink.style.opacity = (blink.style.opacity === 0 ? 1 : 0);
}, 1000);
```


### Obsolete blink HTML ways

What blink HTML ways do not work:

1. &lt;blink&gt; HTML tag itself
2. CSS style `text-decoration: blink;` - does not make your HTML blink


P.S.: Anybody remembers marquee? :D


<style>

.blink {
  animation: 2s linear infinite condemned_blink_effect;
}


@keyframes condemned_blink_effect {
  0% { visibility: hidden; }
  50% { visibility: hidden; }
  100% { visibility: visible; }
}
</style>

 {% include social.html %}