# Connect to a data-source

🖥 To see the code for this step, here is [commit/diff](https://github.com/StrongLoop-Evangelists/band-app/commit/a2c5667cd21ed92f6e5c1f95e3c5e0da43a12dda).

---

In this step, we are going to instantiate a Cloudant DB and connect it to our application. While we will be using a hosted Cloudant DB, most data-sources can be connected in a similar way.

## Cloudant DB on IBM Cloud

Let's jump into the [IBM Cloud catalog](https://console.ng.bluemix.net/catalog/), filter by Databases, find Cloudant and create the service:

![IBM Cloud - Data Apps](assets/bluemix-catalog-data.png)

### Create your service

Once we choose the Cloudant DB service, we are given the option to customize the service. Choose the options as seen in the screenshot below:

![IBM Cloud - Create Cloudant service](assets/cloudant-details-unbound.png)

Clicking create takes us to our Cloudant service instance page:

![IBM Cloud - Cloudant page](assets/cloudant-bm-dash.png)

### Create our database

The next thing we need to do is create the database in our Cloudant service instance. We do so by clicking on the Launch button (shown in screen above) which will launch the Cloudant dashboard for this service instance:

![IBM Cloud - Cloudant page](assets/cloudant-create-db.png)

In the dashboard (shown above), we need to click the Databases option in the left sidebar and then near the top right, click Create Database. We will name the database `band-app`.

Lastly, we need to get the credentials for our Cloudant service -- these credentials are needed in the next step. To do so, we will go back to our IBM Cloud dashboard and from the Cloudant instance overview page, click the 'Service Credentials' tab on the left and click 'New Credential'. Choose a name, and leave the other defaults. Then, press "View Credentials". You'll see your private credentials to access the database - we only need the "url" field.

```
{
  "apikey": "***",
  "host": "***",
  "iam_apikey_description": "***",
  "iam_apikey_name": "***",
  "iam_role_crn": "***",
  "iam_serviceid_crn": "***",
  "port": 443,
  "url": "https://***:***@***-bluemix.cloudant.com",
  "username": "***"
}
```

Make a note of the contents of the URL field - `https://***:***@***-bluemix.cloudant.com`.

## Use Cloudant in our app

### Installation

`cd` into the top-level directory of the LoopBack application and run the following command:

`$ npm install loopback-connector-cloudant --save`

The --save option adds the dependency to the application’s package.json file.

### Add to our LoopBack app

Then we will run `lb datasource` to setup Cloudant for our app. Enter `cloudant` for the data-source name, choose `IBM Cloudant DB` for the connector, use the full connection string URL for the datasource and `band-app` for the database. Leave everything else blank as they are automatically populated from the URL.

```
➜  band-app git:(master) ✗ lb datasource
? Enter the data-source name: cloudant
? Select the connector for cloudantDB: IBM Cloudant DB (supported by StrongLoop)
Connector-specific configuration:
? Connection String url to override other settings (eg: https://username:password@host): https://your-username-here:your-password-here@your-host-here
? database: 
? username:
? password:
? Specify the model name to document mapping, defaults to `loopback__model__name`:
```

### Connect our existing model to Cloudant

And finally, let's change our `event` model to use the Cloudant DB. We can do so by updating our `server/model-config.json`. We'll see a section near the bottom for `event` and will update the object key 'dataSource' to 'cloudant', which is what we called this data-source when adding it to our application above. It should look like this:

```json
  "event": {
    "dataSource": "cloudant",
    "public": true
  }
```

### Test your application

Run `node .` and navigate to `localhost:3000/explorer`. Ensure that you can still POST and GET objects. This time, they are stored in your Cloudant DB on IBM Cloud. If you wanted to, you can verify this by navigating to your Cloudant dashboard from IBM Cloud (where you created the `band-app` database) and ensuring that the entries are there.

*Note: if you are storing your username and password in your code, you will want to be sure to NOT commit them to a public repository.*

## 🚨 SECURITY ISSUE 🚨

We are putting our username and password in our code. If we aren't going to use a public repository, this isn't an issue, but if we do commit this to a public repository, we are making our application vulnerable. There are a number of ways to alleviate this issue. We will look at a preferred method in the next step.

**Next Step:** [Working with credentials](08-credentials.md)
