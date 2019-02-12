---
title: "Simplifying Project Setup on GitHub"
description: "At Coderockr we start and take on many projects, either for clients that hire us or for internal actions, and usually GitHub ends up being the tool we choose for them..."
author: "Lucas Abreu"
date: 2018-06-05
draft: false
categories: ["Desenvolvimento"]
tags: ["Desenvolvimento", "GitHub", "Ferramentas"]
---

At Coderockr we start and take on many projects, either for clients that hire us or for internal actions, and usually GitHub ends up being the tool we choose for them.

Over the years we ended up defining a structure to control our issues, using the following labels:

![Labels set used on [Coderockr Way](https://blog.coderockr.com)](https://cdn-images-1.medium.com/max/2000/1*MQVzTlViaXfd1yrq9PTcWw.png)*Labels set used on [Coderockr Way](https://blog.coderockr.com)*

It is a very simple set, but it makes all the steps, priorities, types and states of the tasks clear, and becomes very easy to understand what is happening.

We register these labels in all of our projects. But even if you’re in a good mood, it’s pretty boring and time-consuming work to register them one by one. And we’re likely to forget some of them, and and so to make sure they are all there you will need to review the list.

![Did I forget the “Stage: Testing”? Oh, found it…](https://cdn-images-1.medium.com/max/2000/1*rkZYym319HTls7CZpoHmYA.gif)*Did I forget the “Stage: Testing”? Oh, found it…*

With this monotonous activity in mind and to save some setup time, we decided to create a script to do this process, adding the labels to a specific project.

The script ended up getting much simpler than expected thanks to the simplicity of the GitHub API, all of it was done with cURL and some bash loops for the labels.

We even prepared it so it does not need to be downloaded/installed, just give a cURL from GitHub and it asks for the data it needs.

![do not even need to download](https://cdn-images-1.medium.com/max/2000/1*cncOsRlkusXuJeYJHnSbVg.png)*do not even need to download*

We decided to leave the script we created in a public repository on GitHub for anyone looking for a similar solution (or who now thinks it’s worth creating one too).

The script and how to use it are [here](https://github.com/Coderockr/coderockr-way-github-setup).

In conclusion, investing a little time to understand the tools you use not only saves you time in the long run, but also generates some cool scripts to share :)

We created a new script to do the same setup for projects in GitLab, you can view the script [here](https://github.com/Coderockr/coderockr-way-github-setup/blob/master/coderockr-way-gitlab-setup.bash)
