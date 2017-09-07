---
layout: post
title:  "git rewrite author"
date:   2015-05-25 18:00:00
categories: git
tags: code gist 
---
To change the name and/or email address recorded in existing commits, you must rewrite the entire history of your Git repository.

Quoted from [github](https://help.github.com/articles/changing-author-info/) :

#### Warning
> This action is destructive to your repository's history.
> If you're collaborating on a repository with others,
> it's considered bad practice to rewrite published history.
> You should only do this in an emergency.

I had to run this script more than once, because I am pretty bad at managing my identities. I added the **'-f'** option

{% gist droptableuser/2ba7bc81251a041bed21 %}
