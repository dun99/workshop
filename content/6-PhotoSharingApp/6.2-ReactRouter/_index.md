---
title: "React Router"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 6.2. </b> "
---

Next we need to create the router in App.js. Our app will have two main routes:

1. A home route - /. This route will render a list of posts from our API
2. A post details route - /post/\:id. This route will render a single post and details about that post.

Using React Router, we can read the Post ID from the route, and then fetch the post associated with it. This is a common pattern in many apps as it makes the link shareable.

Another way to do this would be to have some global state management set up and then set the post ID in the global state. The main drawback of this approach is that the URL cannot be shared.

Other than routing, the main functionality happening in this component is a call to fetch posts from our GraphQL API.

```html
// src/App.js
import React, { useEffect, useState } from "react";

import { withAuthenticator } from "@aws-amplify/ui-react";
import "@aws-amplify/ui-react/styles.css";
import { generateClient } from "aws-amplify/api";

// import query definition
import { listPosts } from "./graphql/queries";

import Button from "./Button";
import CreatePost from "./CreatePost";
import Header from "./Header";
import Post from "./Post";
import Posts from "./Posts";

import "@aws-amplify/ui-react/styles.css";
import { css } from "@emotion/css";
import { HashRouter, Route, Switch } from "react-router-dom";
import { getUrl } from "aws-amplify/storage";

function Router({ user, signOut }) {
  /* create a couple of pieces of initial state */
  const [showOverlay, updateOverlayVisibility] = useState(false);
  const [posts, updatePosts] = useState([]);
  const client = generateClient();

  const [myPosts, updateMyPosts] = useState([]);

  /* fetch posts when component loads */
  useEffect(() => {
    fetchPosts();
  }, []);

  const handleGetUrl = async (key) => {
    const url = await getUrl({
      key,
      options: {
        validateObjectExistence: true,
      },
    });
    return url.url.href;
  };

  async function fetchPosts() {
    /* query the API, ask for 100 items */
    let postData = await client.graphql({
      query: listPosts,
      variables: { limit: 100 },
    });
    let postsArray = postData.data.listPosts.items;
    setPostState(postsArray);

    /* map over the image keys in the posts array, get signed image URLs for each image */
    postsArray = await Promise.all(
      postsArray.map(async (post) => {
        if (post.image) {
          try {
            const imageURL = await handleGetUrl(post.image);
            post.image = imageURL;
            return post;
          } catch (error) {
            if (error.code === "NotFound") {
              console.error("Image not found for post:", post.id);
            } else {
              console.error(
                "Error fetching image URL for post:",
                post.id,
                error
              );
            }
            return post;
          }
        }
      })
    );

    /* update the posts array in the local state */
    setPostState(postsArray);
  }

  async function setPostState(postsArray) {
    const myPostData = postsArray.filter((p) => p.owner === user.username);
    updateMyPosts(myPostData);
    updatePosts(postsArray);
  }

  return (
    <>
      <HashRouter>
        <div className={contentStyle}>
          <Header />
          <hr className={dividerStyle} />
          <Button
            title="New Post"
            onClick={() => updateOverlayVisibility(true)}
          />
          <Switch>
            <Route exact path="/">
              <Posts posts={posts} />
            </Route>
            <Route path="/post/:id">
              <Post />
            </Route>
            <Route exact path="/myposts">
              <Posts posts={myPosts} />
            </Route>
          </Switch>
        </div>
        <button onClick={signOut}>Sign out</button>
      </HashRouter>
      {showOverlay && (
        <CreatePost
          updateOverlayVisibility={updateOverlayVisibility}
          updatePosts={setPostState}
          posts={posts}
          user={user}
        />
      )}
    </>
  );
}

const dividerStyle = css`
  margin-top: 15px;
`;

const contentStyle = css`
  min-height: calc(100vh - 45px);
  padding: 0px 40px;
`;

export default withAuthenticator(Router);


```


