---
title: Managing different SSH keys for GitLab and GitHub on your System.
date: "2022-10-10"
description: "In this blog I will show you how to manage multiple ssh keys on your system"
tags: []
---

###### This article is written assuming that you know how to add ssh keys to your GitHub or GitLab account.

###### I am talking about just GitHub and GitLab but you can use the same for any cloud git providers.

Many times it happens that when you push to your GitHub repository you have to enter your GitHub password each time.
Frustrating, isn't it?

It used to happen a lot with me until I learned how to use ssh with GitHub.

*But now I have got another problem.*😣
I have my code on both GitLab and GitHub and now I have to manage multiple ssh keys but I don't know how and I can't even use the same ssh keys for both of them.😥

Let's see how we can do that 🤠

We will go step by step so be patient before we reach the final step.

Step 1. Generate ssh keys.

```bash
 $ ssh-keygen -C "youremail@email.com"
```

Now that command generates a pair of public and private keys in your `~/.ssh` directory and asks you to provide the name. By default, the name is `id_rsa.pub` and `id_rsa` for public and private keys respectively.

We will generate two pairs of keys each for GitHub and GitLab and name them id_rsa_github and id_rsa_gitlab respectively.

Now your `~/.ssh` should look like

```
|__ id_rsa_gitlab
|__ id_rsa_gitlab.pub
|__ id_rsa_github
|__ id_rsa_github.pub
```

Step 2. Add the respective public key to the respective provider(GitLab and GitHub)

Step 3. _Now comes the important part_. How to make ssh-client know which key to use when pushed to GitHub or GitLab? 🤔

So we can have a `config` file which will help ssh-client to solve the problem.

```bash
$ touch ~/.ssh/config # we are creating a config file using touch utility. You can create the same in whatever way you want
```

Let's see what to put in the config file.

```
Host github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_github

Host gitlab.com
HostName gitlab.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_gitlab
```

Here, we are basically telling ssh-client which ssh private key to use based on the provider.
So whenever we try to push to our GitHub repo it will automatically use the GitHub key and we won't have any trouble handling multiple keys. 😎

If you face any problem in implementing this, let me know in the comments. I Will be happy to resolve.

Thanks for reading 😀
