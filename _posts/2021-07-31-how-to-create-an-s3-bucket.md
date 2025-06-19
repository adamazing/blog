---
title: "How to Create an S3 Bucket"
date: 2021-07-31T17:00:00-12:00
categories:
  - blog
tags:
  - AWS
  - CLI
  - howto
excerpt_separator: <!--more-->
---

![An image of a yellow bucket on a sandy beach](/assets/images/posts/2021-07-31-how-to-create-an-s3-bucket/bucket.webp)

The goal of this article is to guide you through the steps required to set up AWS and the AWS command line interface (CLI) so that by its end you should be comfortable setting up and destroying an [AWS S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html#BasicsBucket) from the command line.

<!--more-->

#### Why Command Line?
It's __absolutely__ possible to log in to the AWS console, navigate the various menus and web forms, and set up an S3 bucket. However, one of the benefits of setting up resources from the command line is that the act now becomes much easier to repeat without error, and much easier to document. If you have to set up multiple resources, being able to script their creation easily will cut down on a lot of laborious and repetitive [Click-Ops](https://www.google.com/search?q=what+is+click-ops).

#### Installing the AWS CLI
Installation of the command line interface itself is relatively simple; AWS provides its own instructions on how to get up and running with its CLI [here](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html). On either macOS or Windows, it's as simple as downloading and running an installer from that page. On Linux, it's as simple as:

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

Whichever platform you're on this should install the aws binary somewhere in your system's `PATH`. To verify successful installation, run `aws --version`; you should see something like:

```
aws-cli/2.2.24 Python/3.8.8 Linux/5.4.0-77-generic exe/x86_64.linuxmint.20 prompt/off
```

#### Configuring AWS Security Credentials

In order to authenticate our AWS command line to our AWS account we're going to use Access Keys. Other methods are available, but are outside the scope of this article.

> **Note**<br /> 
> If you don't already have an account with AWS, you can [sign up for one here](https://portal.aws.amazon.com/billing/signup).
{: .notice--primary}

Once you've signed up for an account and signed in, you should see a menu headed with your name near the top-right of the page; open the menu:

![Screenshot of the AWS account menu](/assets/images/posts/2021-07-31-how-to-create-an-s3-bucket/aws-menu-screenshot.webp){: .align-center}

Click on "My Security Credentials", which should take you to [this page](https://console.aws.amazon.com/iam/home#/security_credentials):

![Screenshot of AWS Console Security Credentials Page](/assets/images/posts/2021-07-31-how-to-create-an-s3-bucket/security-creds-screenshot.webp)

Click on the blue "Create New Access Key" button and a modal will appear:

![Screenshot of the Create Access Key modal](/assets/images/posts/2021-07-31-how-to-create-an-s3-bucket/create-access-key-screenshot.webp)

Click on the "Show Access Key" link and you will be able to see your Access Key ID, and your Secret Access Key. The former acts as an identifier for your account and the latter as a password. You should guard these as anyone in possession of them can do whatever you can do on your account!

With those access keys, it's time to configure the AWS CLI to use them. The easiest way to do this is to go to your command line and run `aws configure`, you will be prompted to enter your access keys as well as a couple of other pieces of information:

![Screenshot of command line after running `aws configure`](/assets/images/posts/2021-07-31-how-to-create-an-s3-bucket/aws-cli-configure-screenshot.webp)

Copy and paste your Access Key ID and Secret Access Key when prompted, then enter a default [region](https://docs.aws.amazon.com/general/latest/gr/rande.html#regional-endpoints), and a [default output format](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html#cli-configure-quickstart-format).

#### Creating A Bucket
Ok, this is what it's all been building toward - creating an S3 bucket.

Still in your command line, run:
```bash
aws s3 mb "s3://my-first-bucket"
```

And, you will (probably) see an error:
![Screenshot of an error. Sad.](/assets/images/posts/2021-07-31-how-to-create-an-s3-bucket/name-collision-error-screenshot.webp)

This is because bucket names have to be globally unique: "[The bucket namespace is shared by all users of the system](https://aws.amazon.com/premiumsupport/knowledge-center/s3-error-bucket-already-exists/)".

Change the command you just ran and give your bucket a unique name; you should see something more like:

![Hurray! Screenshot by Author.Hurrah!](/assets/images/posts/2021-07-31-how-to-create-an-s3-bucket/aws-s3-mb-success-screenshot.webp)

If you need reassurance that you've actually created a bucket, you can navigate to Amazon S3 in the AWS console and check:
 
![Screenshot of AWS Console showing S3 Buckets](/assets/images/posts/2021-07-31-how-to-create-an-s3-bucket/aws-console-bucket-exists-screenshot.webp)


#### Destroying The Bucket
Ok, so you can now delete the bucket - Click-Ops style - from within the AWS console by clicking on the bucket, clicking on the "Delete" button, typing in the bucket name to confirm you __really__ want to delete it, then click the "Delete bucket" button.

Or, you can just run the following command (note, we've just changed `mb` to `rb`):

```bash
aws s3 rb "s3://my-awesome-first-s3-bucket"
```

Et voila:

![Removing the bucket we just created](/assets/images/posts/2021-07-31-how-to-create-an-s3-bucket/aws-s3-rb-success-screenshot.webp){: .align-center}

#### Conclusion

So there we have it. You should now have the AWS CLI tool installed and functioning, and authenticating to your AWS account. Creating and destroying an S3 bucket may not seem that impressive - it's the "Hello World" of infrastructure - but, if you can create an S3 bucket, you can now start using the AWS CLI to do [lots of other things](https://registry.terraform.io/providers/hashicorp/aws/latest/docs).

