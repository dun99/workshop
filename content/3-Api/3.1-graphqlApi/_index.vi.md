---
title: Tạo GraphQL API
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

Để tạo GraphQL API bằng Amplify, ta sử dụng câu lệnh:

```
amplify add api
```

![API](/images/3.api/api-01.png)

Mở file **_amplify/backend/api/photogram/schema.graphql_** bằng IDE hoặc editor của bạn và cập nhật schema như sau:

```
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

Lưu thay đổi và chạy lệnh sau:

```
amplify build
```

![CRA](/images/3.api/api-02.png)
