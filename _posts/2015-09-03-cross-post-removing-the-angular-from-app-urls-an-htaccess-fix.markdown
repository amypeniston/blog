---
layout: post
title: "Cross-post: Removing the Angular # From App URLs: An .htaccess Fix"
modified:
categories: 
excerpt: After approximately 9 gray hairs' worth of search and experimenting, here's how I removed the hash symbol from URLs while allowing routing and page refreshes.
tags: [angular, javascript, routing, hash, url, locationProvider]
image:
  feature:
date: 2015-09-03T09:48:58-03:00
---

Cross-posting this here from my "Web Musings" blog over at [amypeniston.com/web](http://amypeniston.com/web/htaccess-angular-hash-routing) in the hopes that it helps another Angular wannabe.

## Problem

How to remove URL # from Angular apps **and** allow page refreshing.

## Solution

Create a .htaccess at root with the following, replacing `"sub-folder"` with the name of your parent directory. If you're app lives at localhost/web server root, you won't have a parent directory, so remove `"/sub-folder"` from the last line, leaving just `/index.html`.

`RewriteEngine on`      
`RewriteCond %{REQUEST_FILENAME} -s [OR]`       
`RewriteCond %{REQUEST_FILENAME} -l [OR]`      
`RewriteCond %{REQUEST_FILENAME} -d`     
`RewriteRule ^.*$ - [NC,L]`      
`RewriteRule ^(.*) /sub-folder/index.html [NC,L]`

Set `$locationProvider.html5Mode(true);` in your route config.

Define a base URL in your `index.html` that matches your project sub-folder: `<base href="/sub-folder/" />`. If at root, your base will be `<base href="/" />`.

Read about it in more detail over at the [full post](http://amypeniston.com/web/htaccess-angular-hash-routing).