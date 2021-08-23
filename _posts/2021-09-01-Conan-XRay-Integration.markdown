---
layout: post 
comments: false 
title: "Safer C/C++ builds using Conan's XRay integration in Artifactory"
meta_title: "Safer C/C++ builds using Conan's XRay integration in Artifactory"
meta_description: "Safer C/C++ builds using Conan's XRay integration in Artifactory"
---

Xray is a DevSecOps tool that works together with Artifactory to check potential vulnerabilities between the application dependencies. It has support for [multiple package types and different technologies](https://www.jfrog.com/confluence/display/JFROG/JFrog+Xray) (such as Docker images, npm, or PyPI), and since [version 3.21.2](https://www.jfrog.com/confluence/display/JFROG/Xray+Release+Notes) it also supports Conan packages.

In this post, we explain how to make your C/C++ builds secure using Xray with Artifactory. We will go through the setup process using a JFrog free-tier instance that comes with cloud-hosted instances of Artifactory and Xray ready for use with Conan. If you still don't know the JFrog free-tier you can [open an account](https://jfrog.com/start-free/) (it's completely free) to follow the steps in this post. The Artifactory instance has some limitations like a limit of 10GB of transfer a month and 2GB
storage but will be more than enough for personal use or get an idea of how the experience with the JFrog platform is.

If you want to create an instance please [click here to create a new account](https://jfrog.com/start-free/).

## Setting up Artifactory and Xray

Follow this steps adding screen captures (or animated gifs?)

- Add a repo to xray
- Add watches, policies, rules
- Create a rule that blocks downloads, send emails

------
THINGS I MAY USE:

While Xray comes with its own database of software components and vulnerabilities out-of-the-box, it is also open to integration with other databases and tools. Using Xray's open API, customers can integrate Xray with their own systems and data feeds.

What is a Policy?
A Policy enables you to create a set of rules, in which each rule defines a license/security criteria, with a corresponding set of automatic actions according to your needs.

What is a Watch?
A Watch enables you to group selected resources to be scanned, and assign Policies to these Watches for security and compliance.
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