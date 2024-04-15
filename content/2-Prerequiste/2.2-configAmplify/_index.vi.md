---
title: "Cấu hình Amplify"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 2.2 </b> "
---

### Install Amplify CLI

```html
npm install -g @aws-amplify/cli
```

Cấu hình Amplify bằng câu lệnh sau:

```html
amplify configure 

- Specify the AWS Region: us-east-1 || us-west-2 ||
eu-central-1 - Specify the username of the new IAM user: amplify-cli-user > In
the AWS Console, click Next: Permissions, Next: Tags, Next: Review, & Create
User to create the new IAM user. Then return to the command line & press Enter.
- Enter the access key of the newly created user: 
  ? accessKeyId:(<YOUR_ACCESS_KEY_ID>) 
  ? secretAccessKey: (<YOUR_SECRET_ACCESS_KEY>) 
- Profile Name: amplify-cli-user
```

### Khởi tạo Amplify trong ứng dụng Photogram
```html
amplify init
```

![ConnectPrivate](/images/2.prerequisite/amplify-03.png)

Sau khi khởi tạo xong, ta sẽ thấy một thư mục mới được sinh ra có tên là **amplify** trong project, và một file có tên **aws-exports.js** được sinh ra trong thư mục **src**

![ConnectPrivate](/images/2.prerequisite/amplify-04.png)

Để xem trạng thái của Amplify, ta sử dụng câu lệnh:

```html
amplify status
```
![ConnectPrivate](/images/2.prerequisite/amplify-05.png)

Để khởi chạy Amplify console trên browser ta sử dụng câu lệnh:

```html
amplify console
```