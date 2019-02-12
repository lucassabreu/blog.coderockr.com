---
title: "Simplifying Project Setup Even Further"
description: "Last year we created a simple script to assist in the setup of projects in GitHub, it met our needs well for the new repositories we created in GitHub..."
author: "Lucas Abreu"
date: 2018-05-06
draft: false
categories: ["Desenvolvimento"]
tags: ["Desenvolvimento", "GitHub", "Ferramentas"]
---

Now simplifying GitHub, GitLab and Trello

*Em português [clique aqui](https://blog.coderockr.com/simplificando-ainda-mais-o-setup-de-projetos-e6a482b25b8a)*

Last year we created a simple script to assist in the [setup of projects in GitHub](https://blog.coderockr.com/simplifying-project-setup-on-github-1cd2ac2895ee), it met our needs well for the new repositories we created in GitHub.

We also worked with other tools to manage our projects, like GitLab and Trello, but we did not do a similar script for them.

At the beginning of the year, I created a new script to perform the project setup in GitLab, which is basically a copy of the first script, but with the GitLab endpoints..

When I started working on the script for Trello I came across a problem, unlike GitLab and GitHub it does not offer a system of tokens to be used for scripting.

But this does not prevent integrations with Trello, we should just use their authentication process (like OAuth, but much simpler).

I could create a script that uses the token returned by this process, but this would require the user to use the browser to authenticate and then report the script or open the screen during script execution.

I did not like the idea, it looked like a jerry-rig.

Looking for another solution I decided to create a SPA to do the setup of the projects based on Coderockr Way, then when a project setup is necessary, I would not need access to my terminal, I could do it straight from my cell phone, for example.

Once I was making it I decided that the SPA would also cover GitHub and GitLab setups, being a complete solution for the tools we use today.

Then I found another problem, while coding the integration with GitHub and GitLab it would have to use OAuth authentication, which requires a callback and handling from a backend. Meaning that I would also need a server to perform authorizations, GitHub Pages or S3 were not suitable for serving the SPA.

Luckily I already knew another static page deployment service, Netlify! And conveniently it has a feature called [OAuth Providers](https://www.netlify.com/docs/authentication-providers/) that can be used to authenticate against GitHub and GitLab (BitBucket too, but that’s beside the point) as well serving pages over HTTPs which is very good.

Now with all the problems solved, it was only necessary to finish the SPA and publish it 😄

Now, when you need to setup a project, just go to “Coderockr Way Project Setup”, authorize yourself, choose your project in the list (based on your permission) and Apply!

Take a look at how it was: [https://cwps.lucassabreu.net.br](https://cwps.lucassabreu.net.br/)

![](https://cdn-images-1.medium.com/max/2702/1*3i2vq5LRI-jMOamXQQASog.png)

For those who are interested in customizing the tool with the your labels, just make a fork and change the [Labels.js](https://github.com/lucassabreu/coderockr-way-project-setup/blob/master/src/Labels.js) file and enjoy 😉

Here is the source [code](https://github.com/lucassabreu/coderockr-way-project-setup).
