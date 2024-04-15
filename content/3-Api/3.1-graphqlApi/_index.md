---
title: "Adding a GraphQL API"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

To add a GraphQL API, we can use the following command:

```html
amplify add api
```

![API](/images/3.api/api-01.png)

The CLI should open this GraphQL schema in your text editor or IDE. If it doesnt, click on the link provided in the console to see this file **_amplify/backend/api/photogram/schema.graphql_**

Update the schema to the following:
```html
input AMPLIFY {
  globalAuthRule: AuthRule = { allow: public }
}

type Post @model {
  id: ID!
  name: String!
  location: String!
  description: String!
  image: String
}

```

After saving the schema, go back to the CLI and press enter. If Amplify was unable to launch your code editor from the CLI, and you navigated to the schema.graphql file and edited it, you'll need to manually incorporate that schma change by running the following.

```html
amplify build
```

![CRA](/images/3.api/api-02.png)
