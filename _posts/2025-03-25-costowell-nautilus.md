---
layout: post
title:  "Enhancing Nautilus: Simple Improvements for GNOME's File Manager"

categories: 
- Contribution

author: Cole Stowell
---

It's easy to contribute to GNOME! Here's how I did it.

## The GNOME Community

I picked Nautilus, the GNOME project's file manager, for my contribution.
I use Nautilus *every single day* and because I love writing C,
it was the perfect project to start contributing to.

GNOME is a federation type open source community, meaning there is a
massive user base and a massive developer base. They collaborate with
projects like Outreachy and Google Summer of Code to continue bringing
on new, passionate contributors who continue to push out fantastic software.

GNOME puts in sooooo much effort into documentation for all their libraries.
I made use of the [GTK Docs](https://docs.gtk.org/) and
[libadwaita Docs](https://gnome.pages.gitlab.gnome.org/libadwaita/doc/)
which are a quick search away from finding on your own. 
The pages are well formatted and automatically generated from
[GObject Introspection](https://developer.gnome.org/documentation/guidelines/programming/introspection.html),
making them easily maintainable and therefore up-to-date.

For Nautilus in particular, they have a [CONTRIBUTING.md](https://gitlab.gnome.org/GNOME/nautilus/-/blob/main/CONTRIBUTING.md)
which links to a [whole website designed for onboarding](https://welcome.gnome.org/en/app/Nautilus/#getting-the-app-to-build)
and a [whole website detailing commit message guidelines](https://handbook.gnome.org/development/commit-messages.html).

Although they offer a [Discourse Forum](https://discourse.gnome.org/tags/nautilus) to get help and ask questions,
the sheer quality and quantity of documentation made it so I never had to.

## Finding an issue

GNOME's projects have a pretty active Issue Tracker with all kinds of issues. 

Most of them are pretty good about adding the `Newcomer` tag to the issues that
new contributors like myself can get started with some quick contributions.

![Issue](/hfoss2025-blogs/assets/images/costowell/Issue.png)

I picked [issue #3800](https://gitlab.gnome.org/GNOME/nautilus/-/issues/3800) 
from Nautilus, GNOME's file manager, to solve because it looks pretty simple:
You can bookmark a folder more than once.


## Fixing the Bug

The fix was pretty straightforward: check to see if the bookmark exists,
and if it does, return without adding to the list.

Since the bookmark list is a `GList` internally, I use the `g_list_find_custom`
function to iterate over the list and "accept" a bookmark if its location
matches the new bookmark's location. If `g_list_find_custom` returns NULL,
then I know none of the bookmarks matched, so we should add it. Otherwise,
if it's not NULL, it must have already been added so we should return.

I'm not super familiar with GLib or Nautilus's code base so when I made 
my initial PR, I asked for feedback on my function usage. One of the
maintainers responded by saying I should use `nautilus_bookmark_list_contains`,
which hilariously does almost exactly what I implemented.

![Review](/hfoss2025-blogs/assets/images/costowell/Review.png)

I guess I know GLib better than I thought!

Anyway, I adjusted my code, requested a review, and it got merged! Hooray!

![PR](/hfoss2025-blogs/assets/images/costowell/PR.png)

![KindWords](/hfoss2025-blogs/assets/images/costowell/KindWords.png)

## Going Forward

As a happy GNOME user, I am thrilled to start helping out with the project.
Hopefully, I can work on increasingly advanced issues so that I can make
an even more meaningful impact. However, this was a great start and I
plan on continuing to help out!
