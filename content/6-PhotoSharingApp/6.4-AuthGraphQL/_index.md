---
title: "Adding Authorization to the GraphQL API"
date: "`r Sys.Date()`"
weight: 4
chapter: false
pre: " <b> 6.4 </b> "
---

You can update the AppSync API to enable multiple authorization modes.

In this example, we will update the API to use the both Cognito and API Key to enable a combination of public and private access. This will also enable us to implement authorization for the API.
To enable multiple authorization modes, reconfigure the API:

```
amplify update api
```

![API](/images/6-photosharingapp/app-03.png)

Now, update the GraphQL schema to the following:

```
type Post
  @model
  @auth(
    rules: [
      { allow: owner }
      { allow: public, operations: [read] }
      { allow: private, operations: [read] }
    ]
  ) {
  id: ID!
  name: String!
  location: String!
  description: String!
  image: String
  owner: String
}
```

Deploy the changes:

```
amplify push -y
```

Now, you will have two types of API access:

1. Private (Cognito) - to create a post, a user must be signed in. Once they have created a post, they can update and delete their own post. They can also read all posts.
2. Public (API key) - Any user, regardless if they are signed in, can query for posts or a single post.

Using this combination, you can easily query for just a single user's posts or for all posts.

To make this secondary private API call from the client, the authorization type needs to be specified in the query or mutation:

```
await client.graphql({
    query: createPost,
    variables: { input: postInfo },
    authMode: "userPool",
});
```

#### Adding a new route to view only your own posts

Next we will update the app to create a new route for viewing only the posts that we've created.

To do so, first open **_CreatePost.js_** and update the save mutation with the following to specify the authmode and set the owner of the post in the local state:

```
async function save() {
    try {
      const { name, description, location, image } = formState;
      if (!name || !description || !location || !image.name) return;
      updateFormState((currentState) => ({ ...currentState, saving: true }));
      const postId = uuid();
      const postInfo = {
        name,
        description,
        location,
        image: formState.image.name,
        id: postId,
      };

      const result = handleUpload(
        formState.image.name,
        formState.image.fileInfo
      );
      // await uploadData(formState.image.name, formState.image.fileInfo);
      console.log("result", result);
      await client.graphql({
        query: createPost,
        variables: { input: postInfo },
        authMode: "userPool",
      });
      updatePosts([
        ...posts,
        { ...postInfo, image: formState.file, owner: user.username },
      ]); // updated
      updateFormState((currentState) => ({ ...currentState, saving: false }));
      updateOverlayVisibility(false);
    } catch (err) {
      console.log("error: ", err);
    }
  }
```

Next, open **_App.js_**.

Create a new piece of state to hold your own posts named myPosts:

```
const [myPosts, updateMyPosts] = useState([]);
```

Next, in the **_setPostState_** method, update myPosts with posts from the signed in user:

```
async function setPostState(postsArray) {
    const myPostData = postsArray.filter((p) => p?.owner === user?.username);
    updateMyPosts(myPostData);
    updatePosts(postsArray);
  }
```

Now, add a new route to show your posts:

```
<Route exact path="/myposts">
  <Posts posts={myPosts} />
</Route>

```

Finally, open **_Header.js_** and add a link to the new route:

```
<Link to="/myposts" className={linkStyle}>
  My Posts
</Link>
```
