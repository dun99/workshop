---
title: "Deploy API"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

Để deploy API bằng Amplify, ta sử dụng câu lệnh, quá trình deploy sẽ mất vài phút:

```html
amplify push
```

![API](/images/3.api/deploy-03.png)

{{% notice info %}}
Có thể sử dụng câu lệnh amplify push -y để bỏ qua các câu hỏi với lựa chọn Yes
{{% /notice %}}

Deploy xong, giờ ta có thể tương tác với API

## Testing the API

Để test API ta sử dụng AppSync dashboard, để mở AppSync dashboard ta sử dụng câu lệnh, và chọn GraphQL:

```html
amplify console api
```

![API](/images/3.api/deploy-04.png)

Trên màn hình AppSync dashboard, chọn Queries. Ta bắt đầu với việc tạo một bài Post mới:

```html
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

Một bài Post đã được tạo ra, giờ ta có thể truy vấn danh sách toàn bộ bài Post như sau:

```html
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
