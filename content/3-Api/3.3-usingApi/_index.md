---
title: "Using GraphQL with React"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

Now, our API is created & we can test it out in our app!

The first thing we need to do is to configure our React application to be aware of our Amplify project. We can do this by referencing the auto-generated **_aws-exports.js_** file that is now in our **_src_** folder.

To configure the app, open **_src/index.js_** and add the following code below the last import:

```
import { Amplify } from "aws-amplify";
import config from "./aws-exports";
Amplify.configure(config);
```

Now, our app is ready to start using our AWS services

### Interacting with the GraphQL API from our client application - Querying for data

Now that the GraphQL API is running we can begin interacting with it. The first thing we'll do is perform a query to fetch data from our API.

To do so, we need to:

- Define the query
- Execute the query
- Store the returned data in our app state
- List the items in our UI

The main thing to notice in this component is the API call. Take a look at this piece of code:

```
/* Call client.graphql, passing in the query that we'd like to execute. */
import { generateClient } from "aws-amplify/api";
const client = generateClient();
const postData = await client.graphql({ query: listPosts });
```

#### src/App.js

Update your **_src/App.js_** file with the following code, which incorporates the snippet above - calling the GraphQL API

```
import React, { useState, useEffect } from "react";

import { generateClient } from "aws-amplify/api";

import { listPosts } from "./graphql/queries";

export default function App() {
  const [posts, setPosts] = useState([]);
  const client = generateClient();

  useEffect(() => {
    fetchPosts();
  }, []);

  async function fetchPosts() {
    try {
      const postData = await client.graphql({ query: listPosts });
      setPosts(postData.data.listPosts.items);
    } catch (err) {
      console.log({ err });
    }
  }

  return (
    <div>
      <h1>Hello World</h1>
      {posts.map((post) => (
        <div key={post.id}>
          <h3>{post.name}</h3>
          <p>{post.location}</p>
          <p>{post.description}</p>
        </div>
      ))}
    </div>
  );
}
```

In the above code we are using **_client.graphql_** to call the GraphQL API, and then taking the result from that API call and storing the data in our state. This should be the list of posts you created via the GraphQL editor.

Next, test the app - in the terminal type:

```
npm start
```

All the posts displayed here!

![Call API](/images/3.api/react-07.png)
