---
layout: post 
comments: false 
title: "Safer C/C++ builds using Conan's XRay integration in Artifactory"
meta_title: "Safer C/C++ builds using Conan's XRay integration in Artifactory"
meta_description: "Safer C/C++ builds using Conan's XRay integration in Artifactory"
---

Xray is a DevSecOps tool that works together with Artifactory to check potential vulnerabilities between the application dependencies. It has support for [multiple package types and different technologies](https://www.jfrog.com/confluence/display/JFROG/JFrog+Xray) (such as Docker images, npm, or PyPI), and since [version 3.21.2](https://www.jfrog.com/confluence/display/JFROG/Xray+Release+Notes) it also supports Conan packages.

In this post, we explain how to make your C/C++ builds secure using Xray with Artifactory. We will go through the setup process using a JFrog free-tier instance that comes with cloud-hosted instances of Artifactory and Xray ready for use with Conan. If you still don't know the JFrog free-tier you can [open an account](https://jfrog.com/start-free/) (it's completely free) to follow the steps in this post. The Artifactory instance has some limitations like a limit of 10GB of transfer a month and 2GB storage but will be more than enough for personal use or get an idea of how the experience with the JFrog platform is.

If you want to create a free-tier instance please [click here to create a new account](https://jfrog.com/start-free/).

## Setting up Artifactory Conan repository

After loging in the free-tier instance, first thing is creating a new Conan repository in Artifactory. There's a getting started button in the free-tier that guides through the process of creating it. For this post we have created a local repository called *test-repo*. Once we create our new repo we have to configure it in the Conan local client, that's just a matter of executing *conan remote add* and *conan user* commands (you will find detailed instructions in the free-tier getting started guide as well).

## Setting up XRay: adding watches, policies and rules

The first thing we have to define to start working with XRay is a **policy**. A **policy** is just a set of **rules**, and each of these rules defines a license/security criteria that will trigger a corresponding set of actions when met. 

We can create a new policy using the getting started button in the free-tier or just going to *Administration > Xray > Watches & Policies* and creating a new **policy**.

<p class="centered">
    <img src="{{ site.baseurl }}/assets/post_images/2021-09-01/create_new_xray_policy.gif" align="center" alt="Creating a new XRay policy"/>
</p>

We will create a **policy** named *my-company-policy* and add several rules to it. 

First we can create a rules called *low-severity* that will set the minimal severity rule in low (severity score under 4.0/10.0) and that will send a notify email to warn us about that.





Follow this steps adding screen captures (or animated gifs?)

- Add a repo to xray
- Add watches, policies, rules
- Create a rule that blocks downloads, send emails

------
THINGS I MAY USE:

While Xray comes with its own database of software components and vulnerabilities out-of-the-box, it is also open to integration with other databases and tools. Using Xray's open API, customers can integrate Xray with their own systems and data feeds.

What is a Policy?
A Policy enables you to create a set of rules, in which each rule defines a license/security criteria, with a corresponding set of automatic actions according to your needs.

Xray supports two kinds of policies, security and licenses.

What is a Watch?
A Watch enables you to group selected resources to be scanned, and assign Policies to these Watches for security and compliance.


Maybe we can talk about how to create a webhook that creates a Jira ticket for you? https://jfrog.com/blog/jfrog-xray-creating-jira-issues-using-webhooks-in-a-breeze/



SOURCES OF XRAY:

https://www.jfrog.com/confluence/display/JFROG/CVSS+Scoring+in+Xray

Xray collects scores and severities from two sources:

NVD: The National Vulnerability Database (NVD) which contains known vulnerabilities each with their CVSS score.

Security Advisory: Some open source operating systems have their own security trackers with further analysis of the vulnerability inside the operating system package. 


Links:

https://www.jfrog.com/confluence/display/JFROG/CVSS+Scoring+in+Xray

https://www.jfrog.com/confluence/display/JFROG/Creating+Xray+Policies+and+Rules



------

## Back to the conan side

- Do a simple example taking a recipe from conan-center: openssl/1.1.1h that has vulnerabilities.
  Upload it to Arfifactory.
- Screenshots (animated gifs?) of how that looks in Artifactory, show the vulnerabilities.
- Check that it blocks downloads for those artifacts. 

- (Talk about the build integration with Conan? Ask Dima about this. What is the advantage? If we
  had promotions working would it track the whole build regardless if the repo is being tracked in
  xray? Test that with a build that uploads to a repo that is not being scanned by xray)

## Conclusions

- Xray integrates great with conan, very easy to use, you protect dangerous libraries to be
  downloaded to your systems... Check XRay website, take some quotes.