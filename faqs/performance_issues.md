\\n[mininav_troubleshooting](/home/www/zendstudio/dokuwiki/bin/lib/exe/fetch.php?id=&media=mininav_troubleshooting)

(a.k.a. why is my site so slow?)

Normally, Textpattern is one of the fastest CMS platforms around. There
are, however, a few circumstances that can cause performance issues. If
you're experiencing slow page loading, this page should help.

### Runtime vs. Display Time {#runtime-vs.-display-time .sectionedit1#runtime_vs_display_time}

Set the Production Status to
[Testing](/home/www/zendstudio/dokuwiki/bin/doku.php?id=basic_preferences#production_status)
and load a page. Textpattern records the time it takes to generate and
send each page. View the HTML source and scroll to the bottom. You can
check the **runtime** figure to distinguish between slowdowns during
page generation, and those that occur afterwards (i.e. during page
rendering)

On a properly configured web server with a normal load and typical
Textpattern templates and content, expected ranges are:

-   Runtime: 0.01 - 0.5 seconds
-   Queries: 20 - 50
-   Memory: 1500 - 3000 Kb

Values outside these ranges are not necessarily cause for alarm if your
page templates are complex. Also bear in mind that the figures will be
different for each page load, and will vary due to external factors like
the traffic load on your web server. Try loading the page several times
over a period of a few minutes and compare the results. A one-off
anomaly probably means a temporarily overloaded server or network.

If you're experiencing consistently high numbers, here are some things
to check.

#### Runtime

A runtime figure that regularly measures 1 second or more usually
indicates one of a few things:

<ol>
<li>
**DNS issues**: a slow or misconfigured DNS server at your web hosting
company can cause high page runtimes. Usually this occurs in the
Textpattern logging code. In [Advanced
Preferences](/home/www/zendstudio/dokuwiki/bin/doku.php?id=advanced_preferences#use_dns3f),
set 'Use DNS' to 'No', and see if that makes a difference. If not, try
disabling logging altogether

</li>
<li>
**Plugins**: plugins aren't necessarily as efficient as Textpattern
itself. If your performance problems coincide with the installation of a
plugin, or occur only on a particular page template that invokes a
plugin, try disabling it and see if there's a difference. If your page
won't display properly without the plugin, try temporarily reverting to
the default Textpattern [page template and
form](/home/www/zendstudio/dokuwiki/bin/doku.php?id=default_forms</li>)

<li>
<p>
**PHP code**: if you've included any PHP code in your page templates,
whether directly in the template or indirectly via an

</p>
    include()

<p>
call or similar, try disabling it. In particular, check for any code
that might try to fetch a file from an external server, e.g. by using a

</p>
    <nowiki>http://..</nowiki>

<p>
URL in an

</p>
    fopen()

<p>
or

</p>
    include()

<p>
call.

</p>
</li>
<li>
**MySQL issues**: MySQL doesn't have to run on the same physical machine
as your web server. Some hosting companies run these on separate servers
connected by a fast LAN connection, which is fine. However, if
Textpattern and MySQL are on entirely different networks, performance
will be unaviodably slow, since all MySQL queries and results must
travel back and forth over a comparatively slow internet connection
every time a Textpattern page is viewed. Other MySQL performance
problems can be harder to diagnose. An overloaded MySQL server can slow
down Textpattern - ask your hosting company if this could be the case

</li>
<li>
<p>
**Spam blacklist**: sometimes the built-in blacklist checking causes
page slowdowns. Go to [Advanced
Preferences](/home/www/zendstudio/dokuwiki/bin/doku.php?id=advanced_preferences#spam_blacklists_28comma-separated29)
and remove

</p>
    sbl.spamhaus.org

<p>
from the **Spam blacklists** preference if it is there

</p>
</li>
</ol>
#### Queries and Memory {#queries_and_memory}

A high number of queries, or excessive memory usage, is usually caused
by a plugin or custom PHP code. As above, try disabling this code to see
if it makes a difference. Plugins that produce a list of articles are
the most likely culprits: some popular “archive” plugins are very
inefficient, loading the entire database of articles into memory at
once.

### Page rendering {#page-rendering .sectionedit2#page_rendering}

If your Runtime, Queries and Memory figures are all in or near the
normal limits, but you're still experiencing slow page loading, the
problem is almost certainly caused by the content of your page. Some
things to check:

-   **Javascript**: does your page include javascript code? Try
    disabling it
-   **Counters and external stats** : are you using an image or
    javascript link to an offsite “hit counter”, stats service, or a
    local stats application like Shortstat? Try removing the link to see
    if it makes a difference.
-   **Links to off-site objects**: does your page link to images,
    javascript, CSS or other objects on another web server? Do your
    pages include content from external sources such as Gravatars or
    similar? Any of these could be the culprit.
-   **Advertisements**: banner, popup and text ads all work by loading
    content from another server. Try disabling them and measure
    the difference.
-   **CSS**: certain CSS techniques can cause choppy page loading
    and scrolling. In particular, “fixed” background images and blocks
    can cause loading and scrolling problems. You may, however, consider
    installing the [rvm_css](http://vanmelick.com/txp) plugin which can
    improve CSS performance considerably

### Improving performance {#improving-performance .sectionedit3#improving_performance}

If your Runtime/Queries/Memory figures are within normal limits, but
you'd like to improve performance, here are the best methods to use:

<ul>
<li>
**Minimize plugins**: the more active plugins, the more code must be
loaded, parsed and run. Don't use a plugin when you can achieve the same
results with a built-in tag. And make sure you disable any plugins you
no longer use.

</li>
<li>
<p>
**Simplify your code**: do your templates use complex nested
conditionals, PHP code, or forms within forms? Simplify them. In
particular, try to reduce the number of

</p>
    <txp:...>

<p>
tags that require database queries. If you're not sure, remove tags one
at a time and watch the Queries count.

</p>
</li>
<li>
**Caching**: sometimes pages are necessarily complex, and there are
limits to server performance. If you have a popular site that's not fast
enough, try a caching plugin such as
[asy_jpcache](http://forum.textpattern.com/viewtopic.php?pid=54333</li>)

</ul>
The important thing with any performance tweak is to **change only one
thing at a time**, and always measure the results. If something has no
real effect, change it back and try something else. You'll almost
certainly find that one or two changes will have a large effect, while
others will be insignificant.
