---
title: "Fixing GnuPG"
date: "2018-09-27 18:38:24 +0200"
tags: gnupg, gpg
---
Yesterday my GnuPG went sideways with the following message:

![GPG Error message](../files/gpg_broken.jpg)

I ended up trashing my whole installation and reimporting everything before redoing my configuration. The first step is to create a backup of all your keys, in armored base64 format:
{% highlight bash %}
gpg --armor --export > pgp-public-keys.asc
gpg --armor --export-secret-keys > pgp-private-keys.asc
gpg --export-ownertrust > pgp-ownertrust.asc
{% endhighlight %}
I created a backup just to be sure (YMMV, but I like to keep it safe), and then shred and remove it.
{% highlight bash %}
tar cvfz gpg.tgz .gnupg
find .gnupg -type f -exec gshred {} \;
rm -Rf .gnupg
{% endhighlight %}
Now we can reimport everything into the new installation:
{% highlight bash %}
gpg --import pgp-public-keys.asc
gpg --import pgp-private-keys.asc
gpg --import-ownertrust pgp-ownertrust.asc
{% endhighlight %}

I will write an explanatory post for my ```gpg.conf``` later on.
