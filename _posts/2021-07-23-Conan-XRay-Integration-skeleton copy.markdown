---
layout: post 
comments: false 
title: "Safer C/C++ builds using Conan's XRay integration in Artifactory"
meta_title: "Safer C/C++ builds using Conan's XRay integration in Artifactory"
meta_description: "Safer C/C++ builds using Conan's XRay integration in Artifactory"
---

Explain what XRay is and announce that since version XXX it has integration with Conan (maybe mention
that the integration is not full but it's a first step or that will be improved in the future?).

Links to the db's that Xray is using to check for vulnerabilities, give some numbers maybe?

Talk about the free-tier: Did you know that you can create a personal repo with almos all the
functionalities of the JFrog platform?. Explain that we are going to use JFrog free-tier to test the
integration.

## Brief explanation on how xray works and what can be done

Follow this steps adding screen captures (or animated gifs?)

- Add a repo to xray
- Add watches, policies, rules
- Create a rule that blocks downloads, send emails

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