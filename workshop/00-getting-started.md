# What is LoopBack?

🎦 You can view a video walkthrough of this step here: [What is LoopBack: a demo and how to get started](https://youtu.be/Llr18lNuYIo?list=PLxGLihicw5Woe3SV9MCFooTdrI9eOmj54)

---

[LoopBack](http://loopback.io/) is an open-source framework to rapidly build RESTful APIs in Node.js.  It has a command line interface (CLI) tool that you can use to scaffold your application and build out your API layer based on your data (a model-driven approach). It isn't just fast and easy; it's also robust and extensible. It is built on top of Express, so it has widely-used and battle-tested foundation.

**_LoopBack is awesome - checkout this video where LoopBack is used to build fully functional APIs in just 5 minutes:_**

[![Rapid APIs in LoopBack](assets/video.png)](https://youtu.be/Bm6u1f2PUag "Set Up a REST API in 5 Minutes with LoopBack")


## Prerequisites
### Creating an IBM Cloud Account and creating a Kubernetes cluster

In a later step of the lab, you'll be deploying your LoopBack application to a Kubernetes cluster on IBM Cloud. However, spinning up a cluster can take some time. To save some time, let's start by creating an IBM Cloud account, attaching a feature (promo) code, and spawning a Kubernetes cluster. A credit card is not required.

Follow the instructions in the handout you should have received to create your cluster.

#### Node.js and NPM

As mentioned previously, we will need Node.js installed on our local machine for development. [Download Node.js](https://nodejs.org/en/download/).

Included with Node.js is NPM, the Node Package Manager. We will use NPM to install LoopBack in a moment, but first let's confirm that we have Node and NPM. Launch a terminal and run the following commands.

Check for Node:

```
✗ node --version
v8.12.0
```

This should yield a version number. A number greater than 4 should suffice. If you do not have Node installed, see [nodejs.org](https://nodejs.org) for more details on getting it for your platform.

To be safe, let's check NPM as well:

```
✗ npm --version
6.4.1
```

This should also yield a version number; anything greater than 3 is preferable. If you do not have NPM, you likely do not have Node. If so, see above. If you have Node and not NPM, please reinstall Node.

#### LoopBack CLI

Once we have confirmed we have Node.js and NPM, let's install the LoopBack CLI:

```
npm install -g loopback-cli
```

This will install the LoopBack CLI globally (`-g`) to your machine so you can start a LoopBack application in any directory you choose.

Now confirm it's installed:

```
✗ lb --version
4.2.1 (generator-loopback@5.9.4 loopback-workspace@4.5.0)
```

This should show a version number for the LoopBack CLI. 3 or greater for a version number should do.

Type `lb --help` to show all sorts of helpful information:

```
✗ lb --help
Usage:
  lb app [options] [<name>]

Options:
  -h,   --help             # Print the generator's options and usage
        --skip-cache       # Do not remember prompt answers                          Default: false
        --skip-install     # Do not automatically install dependencies               Default: false
        --skip-next-steps  # Do not print "next steps" info
        --explorer         # Add Loopback Explorer to the project (true by default)

Arguments:
  name  # Name of the application to scaffold.  Type: String  Required: false

Description:
  Creates a LoopBack application.

Example:

  lb

  This will create:

    package.json: Development packages installed by npm.

    common/models/<modelName>.json: Definition of basic models provided by LoopBack.
    common/models/: Directory where to put custom model code.

    server/server.js: The main application file.
    server/config.json: Machine-editable app configuration.
    server/datasources.json: Definition of data sources.
    server/model-config.json: Model configuration.

Available commands:

  lb acl
  lb app
  lb boot-script
  lb datasource
  lb export-api-def
  lb middleware
  lb model
  lb property
  lb relation
  lb remote-method
  lb swagger
  lb soap
```

Note those 'Available commands'. We will begin using a number of them shortly.

Next Step: [Let's initialize our application](01-init.md).
