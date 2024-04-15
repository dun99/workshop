---
title: "Tạo ứng dụng React"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 2.1 </b> "
---

Đầu tiên, ta cần tạo một ứng dụng React sử dụng CRA [Create React App CLI](https://github.com/facebook/create-react-app)

```html
npx create-react-app photogram
```

![CRA](/images/2.prerequisite/cra-01.png)

Cd vào thư mục ứng dụng, cài các thư viện cần thiết như aws-amplify, @emotion/css, uuid. react-router-dom@5, @aws-amplify/ui-react

```html
cd photogram npm install aws-amplify @emotion/css uuid react-router-dom@5
@aws-amplify/ui-react
```

![ConnectPrivate](/images/2.prerequisite/cra-02.png)
