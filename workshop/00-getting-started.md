# What is LoopBack?

🎦 You can view a video walkthrough of this step here: [What is LoopBack: a demo and how to get started](https://youtu.be/Llr18lNuYIo?list=PLxGLihicw5Woe3SV9MCFooTdrI9eOmj54)

---

[LoopBack](http://loopback.io/) is an open-source framework to rapidly build RESTful APIs in Node.js.  It has a command line interface (CLI) tool that you can use to scaffold your application and build out your API layer based on your data (a model-driven approach). It isn't just fast and easy; it's also robust and extensible. It is built on top of Express, so it has widely-used and battle-tested foundation.

**_LoopBack is so awesome, it is almost magical. Let me show you by building an API in 73 seconds:_**

[![Rapid APIs in LoopBack](assets/video.png)](https://youtu.be/iOMD27DjuO4 "Rapid APIs in LoopBack")


## Prerequisites
### Creating an IBM Cloud Account and creating a Kubernetes cluster

In this exercise, you'll create an account, attach a feature (promo) code, and create a Kubernetes cluster. A credit card is not required. This workshop will also work with Paid clusters, with a few differences pointed out along the way. Paid clusters come with support for Kubernetes Ingress via an external loadbalancer.

#### Create an IBM Cloud Account

#TODO
1. Visit this [registration link](https://ibm.biz/BdYB5d) and sign up. Follow futher steps with email validation and so on.

2. Get a feature code from this [spreadsheet](https://docs.google.com/spreadsheets/d/1WRJ0CuFRF8bHdUvo0S1EDrGgOrECSpL9gjVUdbOvxQw/edit?usp=sharing). Be sure to mark which one you took so your neighbor doesn't get an error!

3. Click the "User Icon" in the upper right, then in the dropdown select "Profile & Account"

4. On the left select "Billing"

5. Scroll to the middle of the page where there is an 'Apply Code' button for feature codes. Apply the code. 


#### Create a Kubernetes cluster

1. Click the the upper left hamburger menu. Select "Containers". It should be near the top.

2. Select "Create Cluster"

3. If you see a region selection menu, select "US South"

4. Select cluster type "Free"

5. Name your cluster, and press "Create"

You now have kicked off cluster creation. This will take a few minutes - in the meanwhile, you can start locally with LoopBack.

#### Node.js and NPM

As mentioned previously, we will need Node.js installed on our local machine for development. Included with Node.js is NPM, the Node Package Manager. We will use NPM to install LoopBack in a moment, but first lets see if we have Node and NPM:

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
2.2.0 (generator-loopback@3.2.0 loopback-workspace@3.40.1)
```

This should show a version number for the LoopBack CLI. Any version number should do.

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
