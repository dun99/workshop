---
title: "Configure Amplify"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 2.2 </b> "
---

### Installing the CLI

```
npm install -g @aws-amplify/cli
```

Now we need to configure the CLI with our credentials.

```
amplify configure

- Specify the AWS Region: us-east-1 || us-west-2 ||
eu-central-1 - Specify the username of the new IAM user: amplify-cli-user > In
the AWS Console, click Next: Permissions, Next: Tags, Next: Review, & Create
User to create the new IAM user. Then return to the command line & press Enter.
- Enter the access key of the newly created user:
  ? accessKeyId:(<YOUR_ACCESS_KEY_ID>)
  ? secretAccessKey: (<YOUR_SECRET_ACCESS_KEY>)
- Profile Name: amplify-cli-user
```

### Initializing a new project

```
amplify init
```

![ConnectPrivate](/images/2.prerequisite/amplify-03.png)

The Amplify CLI has initialized a new project, and you will see a new folder: **amplify**, as well as a new file called **aws-exports.js** in the **src** directory. These files contain your project configuration.

![ConnectPrivate](/images/2.prerequisite/amplify-04.png)

To view the status of the amplify project at any time, you can run the Amplify status command:

```
amplify status
```

![ConnectPrivate](/images/2.prerequisite/amplify-05.png)

To launch a new browser window and view the Amplify project in the Amplify console at any time, run the console command:

```
amplify console
```
