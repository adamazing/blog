---
title: "Level Up: Infrastructure As Code"
sub_title: "Level 0: What it is, and why you should care."
date: 2021-03-30T17:00:00-12:00
categories:
  - blog
tags:
  - AWS
  - "level up"
  - IaC
excerpt_separator: <!--more-->
---

![Pastels](/assets/images/posts/2021-03-30-level-up-infrastructure-as-code/pastels.webp)

#### What is Infrastructure as Code?
Infrastructure as Code (IaC) is a process by which you can manage and provision the infrastructure your code will run on using machine-readable, committable, reviewable configuration files.

#### Why Infrastructure as Code?
In the beginning, there were servers: actual physical machines that existed in the real world as objects. You could touch them. They lived in a data centre across town, a dingy basement, or, if you were really unlucky, a cupboard shared with the cleaner. If you wanted to install or update something you had to connect to the physical machine over the network or a serial interface. If you wanted to increase the RAM or disk-space, you had to go across town, or down to the aforementioned dingy basement, or go to the cleaning cupboard — dodging the bucket and mop — and open the machine up.

__*Now*__, of course, like [many](https://en.wikipedia.org/wiki/Compact_Disc_Digital_Audio) [other](https://en.wikipedia.org/wiki/VHS) [things](https://en.wikipedia.org/wiki/Banknote) which used to be physical objects, servers are a thing of the past!

Just kidding, of course they’re not, but we live in an age where the complexities of their physical management — which hopefully don’t involve dodging buckets and mops in 2021 — are delegated to a handful of cloud providers, for better or worse. The memory, network throughput, and processor cores of the “machine” running your app can be tweaked up and down and even auto-scale on demand. The future!

So…let’s say you have your spiffy new app, rapidly approaching the end of the [Waterfall development cycle](https://en.wikipedia.org/wiki/Waterfall_model) because I don’t know, whatever, you do you.

You decided upon your cloud provider a while ago and you’ve already set up an EC2 instance and some S3 buckets, all configured and provisioned, and protected by appropriate policies via the AWS Console. You’ve deployed your code to it and you’re happy everything is working. Testers are happy, stake-holders are happy. Now you decide that the time has finally come to set up a second environment, for production.

![Oops! A picture of a young child covering their mouth in shock and regret](/assets/images/posts/2021-03-30-level-up-infrastructure-as-code/uh-oh.webp)

__*Oops*__. You can no longer remember the exact incantation of [Elder Gods’ names](https://en.wikipedia.org/wiki/Cthulhu_Mythos_deities) and which kind of chicken it is that you have to sacrifice in order to divine where AWS has moved the configuration options you need. The steps you documented in That Gist™ are incomprehensible and hopelessly out of date. The AWS for Dummies book with the dog-eared pages whose margins look like John Doe from Se7en’s diaries is nowhere to be found. Jessie, your go-to AWS guru has gone, too.

If only there were a way to have a human and machine-readable configuration file that could live along-side your application code in Git, peer-reviewed and versioned! If __*only*__ there were a way to easily, reliably, and repeatably provision duplicate environments at a whim!

#### Infrastructure as Code to the Rescue

My incredibly contrived example was — somewhat paradoxically — as simplistic as it was hyperbolic. In reality the infrastructure to which you deploy your application may be spread across different cloud providers. It may require configuration of DNS, CDN, lambda functions etc.

At some point it becomes necessary to reduce the cognitive burden associated with that infrastructure setup and give it the same treatment as your application code. Namely, if changes are to be made, they should be:

- tested
- peer-reviewed
- deployed in a deterministic and repeatable manner. 

The responsibility for the infrastructure can then be shared by the whole team. The smooth deployment of your application code no longer relies on someone having remembered to login to AWS in order to re-configure something and all will be right with the world.

#### Next time
We’ll level up and deploy a simple static site to an S3 bucket using [Terraform](https://www.terraform.io/).

