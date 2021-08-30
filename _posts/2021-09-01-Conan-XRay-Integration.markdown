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

## Setting up Artifactory: creating a Conan repository

After loging in the free-tier instance, first thing is creating a new Conan repository in Artifactory. There's a getting started button in the free-tier that guides through the process of creating it. For this post we have created a local repository called *test-repo*. Once we create our new repo we have to configure it in the Conan local client, that's just a matter of executing *conan remote add* and *conan user* commands (you will find detailed instructions in the free-tier getting started guide and in [Artifactory documentation](https://www.jfrog.com/confluence/display/JFROG/Conan+Repositories)).

## Setting up XRay: adding watches, policies and rules

The first thing we have to define to start working with XRay is a **policy**. A **policy** is just a set of **rules**, and each of these **rules** defines a license/security criteria that will trigger a corresponding set of actions when met. 

We can create a new policy using the *Getting Started* button in the free-tier or just going to *Administration > Xray > Watches & Policies* and creating a new **policy**.

<p class="centered">
    <img src="{{ site.baseurl }}/assets/post_images/2021-09-01/create_new_xray_policy.gif" align="center" alt="Creating a new XRay policy"/>
</p>

We will create a security **policy** named *mycompany-policy* and add one rule to it.

First we can create a rule called *low-severity-warning* that will set the minimal severity rule in low (severity score under 4.0/10.0) and that will send a notify email to warn us about that. For this, you just have to click on *New Rule* and setting the Minimal Severity. You can set the severity warning based on pre-defined ranges (low, medium, high or critical) or set a custom CVSS Score range. Please [read more about this](https://www.jfrog.com/confluence/display/JFROG/CVSS+Scoring+in+Xray) in XRay docs. As you can see there are multiple actions that can be triggered when the rule conditions are met. This time we will only set the *Notify Email* field and see what happens when we upload a package that has kwown security issues.

<p class="centered">
    <img src="{{ site.baseurl }}/assets/post_images/2021-09-01/xray_rules_options.png" align="center" width="50%" alt="Creating a new XRay rule"/>
</p>

Now that we have created our policy that has just one rule we will add a **watch**. A **watch** is the piece that connects the **resources** to be scanned with the **policies**. To create the **watch** you could use the *Getting Started* button or going to *Administration > Xray > Watches & Policies* and selecting the *Watches* tab.
We will create a *test-watch* that will add our *test-repo* as resource (you could also use patterns for repository inclusion or add all the repositories to the watch). Then click on *Manage Policies* to connect the *mycompany-policy* repo to the *test-repo* resource. 

<p class="centered">
    <img src="{{ site.baseurl }}/assets/post_images/2021-09-01/create_new_xray_watch.png" align="center" alt="Creating a new XRay watch"/>
</p>

## Testing with the Conan client

Now that we have added a our **policy** with the notify e-mail **rule** and that we have connected the Conan repository to that **policy** adding a **watch** we are going to test that uploading a Conan package that's affected by some vulnerability to the repo to check that everything is working fine. Let's try for example with *openssl/1.1.1h* that should be affected by [CVE-2020-1971](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1971).

```
conan install openssl/1.1.1h@ -r conancenter
conan upload openssl/1.1.1h --all -c -r test-repo
```

Just right after this package was uploaded you should have received an e-mail warning about the policy violation:

<p class="centered">
    <img src="{{ site.baseurl }}/assets/post_images/2021-09-01/xray_warning_email.png" align="center" alt="XRay policy break warning email"/>
</p>

Clicking in the link of the email and selecting the *XRay Data* tab will take you to all the details about the vulnerabilities present in the package.

<p class="centered">
    <img src="{{ site.baseurl }}/assets/post_images/2021-09-01/xray_openssl_report.png" align="center" alt="XRay policy break warning email"/>
</p>

Now we will modify the rule we previously added and also check the Block Download option. Now if we try to install any binary that's affected by security issues XRay should block the download.

<p class="centered">
    <img src="{{ site.baseurl }}/assets/post_images/2021-09-01/xray_blocking_downloads.gif" align="center" alt="XRay blocking downloads of insecure artifacts"/>
</p>

This is just a simple example of what you can do to make your C/C++ builds more secure using Conan together with Artifactory and XRay, but you can experiment with other options such as 



---> ADD SCREENSHOT OF THE RULE????





Follow this steps adding screen captures (or animated gifs?)

- Add a repo to xray
- Add watches, policies, rules
- Create a rule that blocks downloads, send emails, more options: use webhooks to push notify to slack, create a jira ticket...

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