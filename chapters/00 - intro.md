
# Intro

jQuery is holding you back.

It slows down your sites, especially on mobile devices and spotty connections. jQuery 3, minified, is *only* 82kb, which might not seem big, especially if you regularly have access to a fast internet connection. But for your visitors who are on older devices, slower connections, or spotty mobile networks, a hanging script can cripple your website.

If you host jQuery from a CDN and the file fails to load, it can complete break your site. This actually happened back in 2013 OpenDNS temporarily blocked access to Google-hosted files and broke a ton of sites in the process.^[[http://www.itsadam.co.uk/google-hosted-jquery-or-other-scripts-not-working/](http://www.itsadam.co.uk/google-hosted-jquery-or-other-scripts-not-working/)]

And using jQuery forces you to choose between limiting the frameworks you work with or adding additional performance-crushing dependencies to your site.


## Abstraction isn't always a good thing

jQuery used to smooth out weird inconsistencies between browsers. That's no longer the case.

Today, modern JavaScript and browser APIs are as easy to use as jQuery and work consistently across browsers. And jQuery 3 only supports Internet Explorer 9 and up, so you don't even gain any backwards compatibility.

It *does* provide some abstraction for certain tasks, which allows you to right fewer lines of code.

**But...** that abstraction hides what's really happening under the hood and is more forgiving of mistakes, which makes it easier for you to write bad code. And writing a few less lines of code is kind of pointless if you have to download an additional 10,000 lines of code to do it.

This book will teach you how JavaScript (and the browser) really work. In the process, you’ll become a better developer and boost your career.

## How to ditch jQuery

In the next chapter, we'll look at vanilla JavaScript equivalents of common jQuery APIs. As you'll see, a lot of modern vanilla JS APIs work in much the same way that jQuery APIs do.

Next, we'll look at how to easily ensure cross-browser compatibility and provide access to all browsers (even Netscape and IE 5). This used to be one of the biggest reasons to use jQuery, but it's far easier today.

After that, we'll get into some JavaScript techniques and best practices. Things like how to properly scope your code, and how to debug it when things go wrong.

The best way to learn vanilla JavaScript, though, is to actually write code.

In the last part of this book, we'll work on two vanilla JS projects:

1. Invisible Ink, an expand-and-collapse/accordion script.
2. Play, a YouTube video player.

Let's get started!