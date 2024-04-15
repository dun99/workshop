---
title: "Authentication in React"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

#### Using the withAuthenticator component

To add authentication in the React app, we'll go into **_src/App.js_** and first import the **_withAuthenticator_** HOC (Higher Order Component) from **_@aws-amplify/ui-react_**:

```html
// src/App.js, import the withAuthenticator component and associated CSS
import { withAuthenticator } from "@aws-amplify/ui-react";
import "@aws-amplify/ui-react/styles.css";

```

Next, we'll wrap our default export (the App component) with the withAuthenticator HOC:

```html
function App() {
  /* existing code here, no changes */
}

/* src/App.js, change the default export to this: */
export default withAuthenticator(App);


```

Now we can run the app and see that an Authentication flow has been added in front of our App component. This flow gives users the ability to sign up and sign in.

![API](/images/4-auth/auth-04.png)

Click "Sign Up" and follow the prompts to create an account. Be sure to use a real email address! Once you submit your user information, check your email for a confirmation email to complete the sign up.

Now that you have the authentication service created, you can view it any time in the console by running the following command - select User Pool:

```html
amplify console auth

Using service: Cognito, provided by: awscloudformation
? Which console
❯ User Pool
Identity Pool
Both
```

#### Add sign out button

You can also easily add a preconfigured UI component for signing out. First, modify the App function signature.

```html
function App({ signOut, user }) {
  ...
   <h1>Hello World</h1>
  ...
  <button onClick={signOut}>Sign out</button>
}
```

Add some styling

Next, let's update the UI component styling. Open ***src/index.css*** and add the following styling:

```html
:root {
  --amplify-primary-color: #006eff;
  --amplify-primary-tint: #005ed9;
  --amplify-primary-shade: #005ed9;
}
```

See the result, you can click Sign out button to Sign out

![API](/images/4-auth/auth-05.png)

#### Accessing user data

We can access the user's info now that they are signed in by calling ***currentAuthenticatedUser()*** in ***useEffect***. Add the following code to ***src/App.js*** in the appropriate places

```html
import { getCurrentUser } from "aws-amplify/auth";
...

useEffect(() => {
    fetchPosts();
    currentAuthenticatedUser();
  }, []);

async function currentAuthenticatedUser() {
    try {
      const { username, userId } = await getCurrentUser();
      console.log(`The username: ${username}`);
      console.log(`The userId: ${userId}`);
    } catch (err) {
      console.log(err);
    }
  }
```

After saving these changes and reloading, you should see user information logged in the Developer Tools console of your browser.

![API](/images/4-auth/auth-06.png)
