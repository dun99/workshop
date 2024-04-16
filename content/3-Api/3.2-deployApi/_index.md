---
title: "Deploying the API"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

## Deploying the API

To deploy the API, run the push command:

```
amplify push
```

![API](/images/3.api/deploy-03.png)

{{% notice info %}}
Alternately, you can run amplify push -y to answer Yes to all questions.
{{% /notice %}}

Now the API is live and you can start interacting with it!

## Testing the API

To test it out we can use the GraphQL editor in the AppSync dashboard. To open the AppSync dashboard, run the following command:

```
amplify console api
```

![API](/images/3.api/deploy-04.png)

Alternatively, you can just navigate to the AppSync dashboard in the AWS console and search for your postagram-dev application.

In the AppSync dashboard, click on Queries to open the GraphiQL editor. In the editor, create a new post with the following mutation:

```
mutation createPost {
  createPost(
    input: {
      name: "My first post"
      location: "New York"
      description: "Best burgers in NYC - Jackson Hole"
    }
  ) {
    id
    name
    location
    description
  }
}
```

![API](/images/3.api/deploy-05.png)
Then, query get all the posts:

```
query listPosts {
  listPosts {
    items {
      id
      name
      location
      description
    }
  }
}
```

![API](/images/3.api/deploy-06.png)
