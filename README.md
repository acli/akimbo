This is a script to poll Akimbo (a site used by Canadian artists) to display
a list of calls for submissions, together with their indicated deadlines.
The calls are sorted by deadline,
so the most urgent calls are listed first.

This script was written mainly because it’s very difficult to figure out what the deadlines are using Akimbo’s interface.
It is also impossible to filter out listings that are past their deadlines using their UI.

How do I know the deadlines?
============================

If you’ve ever looked at Akimbo’s RSS feed or given some thought to what fields are on their posting pages,
you know there’s no deadline field.

However, they do *try to* mark up deadlines with a CSS class `text-deadline` (usually with `<span class="text-deadline">`).
So we can examine the page and look for these marked-up text snippets.
There are only two problems:

The first is the dates are all in English.
You have to parse the English to figure out what the actual date and time is,
so obviously sometimes you’re not sure,
and most of the time you have no idea what time zone is implied.

The second is *sometimes* there are deadlines that are *not* marked with the `text-deadline` class.
This just looks like whoever entered the posting forgot their own house style.
In this case there’s nothing we can do.

What is treated as a call?
=========================

Akimbo has an option to list only art calls.
In theory we can poll this list and we’ll get 100% of the art calls and 100% of what we get will be art calls. 
Unfortunately, in reality some calls are listed under other “types” such as learning experiences
(this is often true for residencies)
or just generic events.
As a result the script just polls the RSS feed, which contains all types of events.

To reduce the amount of noise,
all listings without a deadline
are assumed to be non-calls and removed from the display unless you specify the `-A` (or `--almost-all`) option..

Impact on server load
=====================

The script is designed to be *very* gentle on the Akimbo server.
It should load the server even less than an ordinary graphical web browser
since only the text is loaded, not any graphics, style sheets or scripts.

Only one page of the listing is loaded (it will not click Next)
and the listing is loaded only every hour.
(It caches the listings page and will not load a new copy until an hour has passed.)

Details of the calls (including deadlines)
require loading individual posts
(just like we have to control-click each post to open individual posts on a graphical web browser),
but these loads are always accompanied by a delay to further reduce server load.
Posts are cached so every post will only be loaded once unless their URL changes.

Bugs
====

Time zone “support” is entirely wrong.
In particular, not all zones are recognized,
the user is assumed to be in the Eastern time zone,
and the script doesn’t know how to deal with the Atlantic time zone.

Paths of the script’s dependencies like *file*(1) or *wget*(1) are all hard-coded.

The `--dry-run` option doesn’t prevent all actions.

Dependencies
============

- *file*(1)
- *wget*(1)
- *zcat*(1) (from *gzip*)

