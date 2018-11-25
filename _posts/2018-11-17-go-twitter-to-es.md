---
layout: default
title: "Indexing your liked tweets into ElasticSearch"
date: "2018-11-17 20:20 +0100"
categories: programming
tags: go elasticsearch
---

Ever thought you'd want to actually get some value out of your twitter likes? 
I am using it as bookmarking service and as a drop in replacement for "read it later". 

Sadly the platform does not offer any easy way to find your likes again. 
To retrieve my Likes from the platform initially I use this [IFTTT Recipe](https://ifttt.com/applets/rEwKaV8X-archive-tweets-you-like-to-a-google-spreadsheet) 
to add it into a spreadsheet, which I eventually download as a comma-separated CSV. 

Since this saves the link to the original tweet, it is possible to bulk retrieve them 
at a later point.

This is what my [project](https://github.com/droptableuser/go-twitter-to-es) will do. 
It is written in go and puts all your saved likes into an ElasticSearch. 