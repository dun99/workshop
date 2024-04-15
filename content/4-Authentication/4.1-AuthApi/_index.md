---
title: "Add Authentication"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

Next, let's update the app to add authentication.
To add the authentication service, we can use the following command:

```html
amplify add auth
```

![API](/images/4-auth/auth-01.png)

To deploy the authentication service, you can run the push command:

```html
amplify push
```

![API](/images/4-auth/auth-02.png)

When this step completes you will have authentication services set up in Amazon Cognito. To see more information, you can run the console command:

```html
amplify console auth
```

Choose User Pool

![API](/images/4-auth/auth-03.png)
