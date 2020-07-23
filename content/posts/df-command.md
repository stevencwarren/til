---
title: "Df Command"
date: 2020-07-21
draft: false
summary: Today we worked through a deployment issue where the deployments failed due to the server running out of disk space. This is how we used the df command to resolve the issue.
tags:
  - devops
---

Earlier today we had an issue deploying a front-end application. The deployment process is a bit peculiar since we use Netlify to build the application and then copy the source file to a legacy server. I found out later that this was a half-done migration. Eventually, the site will be wholly served by Netlify.

Our developer deployed the app, Netlify said the build succeeded, and our chatbot returned a successful build. But, unfortunately, the code was not in the client.

To debug this, I first downloaded the master branch and ensured that the code was merged correctly. Then I verified that the build on Netlify contained the latest code. Lastly, I figured the failure must have been on the custom server.

When I logged in to the server, every time I tried to cd to a directory I saw this error:

```
bash: cannot create temp file for here-document: No space left on device
```
So, I consulted Dr. Google and [found a great article that explained](https://www.ostechnix.com/the-df-command-tutorial-with-examples-for-beginners/) `df`.

After running df, I found out that 99% of the disk was being used and the deploy couldn't be written to the disk.

I chatted with our platform team and they cleared out the Nginx logs and we were able to deploy again.
