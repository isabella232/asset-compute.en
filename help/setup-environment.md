---
title: Set the development environment required for Asset Compute Service.
description: Developer environment setup for Asset Compute Service to start creating and testing custom code.
---

# Setup a developer environment {#create-dev-environment}

To create a setup that allows you to develop for Compute Service, follow these requirements and instructions.


1. [Acquire access and credentials](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/setup.md#acquire-access-and-credentials) for Project Firefly.

2. [Set up the local environment](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/setup.md#local-environment-set-up) and the required tools.

3. Some more tools that help you get started developing smoothly are:

   * [Git](https://git-scm.com/).
   * [Docker Desktop](https://www.docker.com/get-started).
   * [NodeJS](https://nodejs.org) (v10 to v12 LTS, odd versions are not recommended) and [NPM](https://www.npmjs.com). User of OSX HomeBrew can do `brew install node` to install both. Otherwise, download it from the [NodeJS download page](https://nodejs.org/en/). 
   * An IDE that is good for NodeJS, we recommend [Visual Studio Code (VS Code)](https://code.visualstudio.com) as it is the supported IDE for the debugger. You can use any other IDE as a code editor, but advanced usage (e.g. debugger) is not yet supported.
   * [AIO CLI](https://github.com/adobe/aio-cli) (`aio`) - install using `npm install -g @adobe/aio-cli`.

4. Ensure to meet the [prerequisites](release-notes.md#prerequisites).

# Setup a Firefly Project {#create-firefly-project}

1. Be granted System Admin or Developer Role access in the Experience Organization (This can be set by a System Admin in the [Admin Console](https://adminconsole.adobe.com/overview).)
2. Log onto the [Adobe Developer Console](https://console.adobe.io/)
    - Make sure to be in the same Adobe Experience Cloud Organization as the AEM as a Cloud Service Integration
    - _For more information about Adobe Developer Consolse, please refer to [Console Documentation](https://www.adobe.io/apis/experienceplatform/console/docs.html)_
3. Create a new Project Firefly project (_Full instructions [here](https://www.adobe.io/apis/experienceplatform/project-firefly/docs.html#!AdobeDocs/project-firefly/master/getting_started/first_app.md)_)
    - Click `"Create new project" => "Project from template"` and choose `"Firefly"`
    - This will create a new Firefly Project with two workspaces: `Production` and `Stage`. Feel free to add additional workspaces (e.g. `Development`)
4. Inside the Firefly Project, choose a workspace and subscribe to the services needed for Asset Compute:
    - Click on `"Add to Project" => "API"` and add each of these services: `"Asset Compute"`, `"IO Events"`, `"IO Events Management"`
    
    _While adding the first API, you will be prompted to create a private key. Please save this somewhere safe on your machine. You will need this for tesing your custom worker with the developer tool._


Update this section with the following information:

* Any steps in the beginning that lead to gotchas later should be called out for caution? For example,
  * don't change some defaults initially
  * know risks when deviating from standard path
  * naming conventions to follow
  * Retrieve and format credentials (YAML file details)

## Next Step {#next-step}

Now that your environment is set up, you are ready to [create a custom worker](./develop-custom-worker.md).