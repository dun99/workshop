---
title: "Create React App"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 2.1 </b> "
---

To get started, we first need to create a new React project using the [Create React App CLI](https://github.com/facebook/create-react-app)

```
npx create-react-app photogram
```

![ConnectPrivate](/images/2.prerequisite/cra-01.png)

Now change into the new app directory and install NPM packages for AWS Amplify, AWS Amplify UI React, react-router-dom, emotion, and uuid

```
cd photogram
npm install aws-amplify @emotion/css uuid react-router-dom@5 @aws-amplify/ui-react
```

![ConnectPrivate](/images/2.prerequisite/cra-02.png)
