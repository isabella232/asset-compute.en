---
title: Deploy Asset Compute Service custom worker.
description: Deploy Asset Compute Service custom worker.
---

# Deploy and publish a custom application {#deploy-custom-worker}

**TBD:**

* Document how to deploy and publish to production.
* Replace all git.corp links with publicly-available links.

## Release your Docker image

If your worker has a special Docker image that it uses that image needs to be published to the appropriate repository. This currently is a manual process. To do this do the following:

1. Go to [ci-deploy-runtime](https://nui.ci.corp.adobe.com/blue/organizations/jenkins/ci-deploy-runtime/activity) in Jenkins
1. Make sure you are logged in
1. Click on the `Run` button above the `STATUS` column.
1. Click on the circle right below the `STATUS` label.
1. Under where it says `Provide Docker runtime to push` there are 3 text boxes
1. In the first enter the name of your worker
1. In the second enter the directory where the Docker file resides, typically either `runtime` or `docker`
1. In the third leave the branch set to `master`
1. Click on the `PUSH` button

## Release your action

There is a script named `nui-release.sh` which resides in [infra/scripts](https://git.corp.adobe.com/nui/infra/blob/master/scripts/nui-release.sh).  From your `action` directory first run `nui-release.sh`.  That does a dry run.  If that looks good then run `nui-release.sh do` which will update the version and publish it.
