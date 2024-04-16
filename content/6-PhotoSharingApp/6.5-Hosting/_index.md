---
title: "Hosting"
date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: " <b> 6.5 </b> "
---

The Amplify Console is a hosting service with continuous integration and deployment.

The first thing we need to do is create a new GitHub repo for this project. Once we've created the repo, we'll copy the URL for the project to the clipboard & initialize git in our local project:

```
git init

git remote add origin git@github.com\:username/project-name.git

git add .

git commit -m 'initial commit'

git push origin master

```

Next we'll visit the Amplify Console for the app we've already deployed:

```
amplify console
```

In the Hosting Environments section, under Host a web app choose GitHub then click on Connect branch.
![Deploy](/images/6-photosharingapp/app-04.png)

Authorize Github as the repository service.

Next, we'll choose the new repository & branch for the project we just created & click Next.
![Deploy](/images/6-photosharingapp/app-05.png)

In the next screen, we'll create a new role & use this role to allow the Amplify Console to deploy these resources, Choose dev Environments & click Next.
![Deploy](/images/6-photosharingapp/app-06.png)

Finally, we can click Save and Deploy to deploy our application!
![Deploy](/images/6-photosharingapp/app-07.png)

Now, we can push updates to Main to update our application. The deployment process may take a few minutes.
![Deploy](/images/6-photosharingapp/app-08.png)

Deploy success!!! You can see our app in new domain
