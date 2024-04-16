---
title: "Sử dụng API"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

API đã được tạo, giờ ta có thể tương tác với chúng trên ứng dụng ReactJS

Đầu tiên, ta cần phải cấu hình cho ứng dụng React có thể tương tác với GraphQL API. Để cấu hình, ta thêm vào file **_src/index.js_** đoạn import dưới đây. Trong đó file **_aws-exports.js_** đã đauto-generated trong thư mục **_src_**

```
import { Amplify } from "aws-amplify";
import config from "./aws-exports";
Amplify.configure(config);
```

Giờ đây, ứng dụng đã sẵn sàng sử dụng dịch vụ AWS

### Tương tác với GraphQL

Bây giờ GraphQL API đã hoạt động, chúng ta có thể bắt đầu tương tác với chúng. Đầu tiên chúng ta sẽ thực hiện một truy vấn để lấy dữ liệu từ API.

Để làm điều đó, ta cần:

- Xác định truy vấn
- Thực thi truy vấn
- Lưu dữ liệu được trả về trong trạng thái ứng dụng của chúng ta
- Liệt kê các mục trong giao diện người dùng của chúng ta

Dưới đây là cách truy vấn dữ liệu, lấy tất cả các bài post từ API

```
/* Call client.graphql, passing in the query that we'd like to execute. */
import { generateClient } from "aws-amplify/api";
const client = generateClient();
const postData = await client.graphql({ query: listPosts });
```

#### src/App.js

Update file **_src/App.js_** như sau:

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
      ))}[text](https://us-east-1.console.aws.amazon.com/appsync/home?region%3Dus-east-1#%2F)
    </div>
  );
}
```

Đoạn code trên ta đã sử dụng **_client.graphql_** để truy vấn dữ liệu từ GraphQL API, sau đó lưu dữ liệu vào một state và hiển thị chúng. Tất cả các bài post đã được tạo ra trước đó thông qua AWS AppSyncs ở phần trước.

Kiểm tra kết quả:

```
npm start
```

Tất cả bài post đã được hiển thị!

![Call API](/images/3.api/react-07.png)
