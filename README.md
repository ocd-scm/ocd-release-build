# ocd-release-build

This is a webhook chart to perform a release build on Openshift (aka OKD). The idea is that it creates a tagged release image that exactly matches the code that has the git release tag. 

As this component is used for release builds it can run along side any CI build. For example, we use circleci for our CI build. We then use the OCD slackbot to trigger a git release from a branch, tag or commit. A webhook then fires this component running in openshift that makes our release build tagging the resulting image with the git release tag.

# Architecture

This component requires quite a lot of internal moving parts. The good news is that it is supplied as a simple to configure chart. It also uses stable and standard Origin features that work on locked down Origin. It would have been simpler to use a custom build type but that has to be enabled by a cluster administrator. The internal complexity of this component is to avoid needing cluster admin rights. 

![alt text][ocd-build-components]

[ocd-build-components]: https://github.com/ocd-scm/ocd-meta/blob/master/imgs/ocd-webhook.png?raw=true "OCD Release Build Components"

It is implemented using the awesome [webhook engine](https://github.com/adnanh/webhook/blob/master/webhook.go) which is a small but beautful Go app that is configured to match and extract webhook json to invoke shell script. The shell script then logs into openshift and patches the git tag into the build config. 

## Usage

The [ocd-meta wiki](https://github.com/ocd-scm/ocd-meta/wiki) has a full tutorial on each of Minishift and Openshift Online Pro that includes setting up a release build environment. That uses Helmfile to install this component.
