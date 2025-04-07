---
layout: post
title:  "tldr pages: adding recon-ng page"
categories: 
- Contribution
author: Andrew Klein
---


## The Tldr Pages Community

I picked [Tldr Pages](https://tldr.sh/), a community that simplifies the typical man pages and provides examples of commands typically used. The project covers commands used on various systems, including: UNIX, Linux, macOS, SunOS, Android and Windows. The project also has an in-browser app to demo the tldr pages as well.

I picked this project because it looked like it had an active community, serves useful purpose, and looked relatively easy to contribute to. The project was very active as there were many pull requests with the maintainers actively review the requests, making suggestions, and adding the requests to the main branch.

This project has a great landing page that goes over what the project is, how to install/use the project, how to contribute to the project, and similar/related projects. When it comes to project contribution the give examples of what someone can do to contribute to the project and then provides links to community resources and the contribution guidlines: [CONTRIBUTING.md](https://github.com/tldr-pages/tldr/blob/main/CONTRIBUTING.md). This guidelines file gives the whole rundown on what files should contain, how to format them, and how to submit a pull request.

## Finding an Issue

For finding how I wanted to contribute I first went to the issues tab and looked to see if they had a `good first issue` tag. They did so I filtered through and most of them were translation requests but then I found an issue for documenting penetration testing tools.

![Issue](/hfoss2025-blogs/assets/images/ajk1686/issue-pig.JPG)

Here is the exact issue: [Issue 6863](https://github.com/tldr-pages/tldr/issues/6863). In this issue was a comprehensive list of penetration testing tools that need documentation to be added. I scrolled thought the list until I found one that I wanted to add documentation for. I chose to add documentation for the tool 'recon-ng'. Recon-ng is a reconnaissance tool that uses automation to scour the web for information on a target. 

## Adding the page

I first researched the tool on google and youtube to find out the basics of the tool and common commands that a user would use. I then made a fork, adding the page and started with a brief description of the tool and a link to the wiki for the tool. I then added commands that a user would enter to begin a session and used a common module for example uses of the tool. I then committed my changes and submitted a pull request: [Pull Request #16121](https://github.com/tldr-pages/tldr/pull/16121).

![Pull Request](/hfoss2025-blogs/assets/images/ajk1686/pr.JPG)

After submitting the pull request, a bunch of tests are run, one of them makes sure that the submitting user has signed the contributor license agreement. At first I ran into errors due to the tests wanting colons at the end of sentences instead of periods. I committed the changes and the tests were ran again, but there were still errors. The tests wanted me to change the wording from present tense to infinitive tense. I fixed these issues and committed the changes, this time the test all passed.

![Error Fixes](/hfoss2025-blogs/assets/images/ajk1686/updates.JPG)

I am now waiting for the maintainers to review the changes and accept the pull request.

## For the Future

I would definitely be interested in contributing more to this project as I think the project is very helpful for users and there are lots of pen testing tools that need documentation. I know now the proper style and wording that the project wants for contributions so any contributions should be much easier in the future.
