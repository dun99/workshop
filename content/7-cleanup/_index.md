---
title: "Clean up resources"
date: "`r Sys.Date()`"
weight: 7
chapter: false
pre: " <b> 7.</b> "
---

#### Removing Services

If at any time, or at the end of this workshop, you would like to delete a service from your project & your account, you can do this by running the amplify remove command:

```
amplify remove auth

amplify push

```

If you are unsure of what services you have enabled at any time, you can run the amplify status command:

amplify status

amplify status will give you the list of resources that are currently enabled in your app.

#### Deleting the Amplify project and all services

If you'd like to delete the entire project and all of the resources associated with it, you can run the delete command:

```
amplify delete
```
