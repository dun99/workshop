---
title: "Xác thực trong React"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

#### withAuthenticator component

Để thêm tính năng xác thực vào ứng dụng React, chúng ta sẽ vào **_src/App.js_** và import **_withAuthenticator_** HOC (Higher Order Component) từ **_@aws-amplify/ui-react_**:

```html
// src/App.js, import the withAuthenticator component and associated CSS
import { withAuthenticator } from "@aws-amplify/ui-react";
import "@aws-amplify/ui-react/styles.css";
```

Tiếp theo, bọc App component với withAuthenticator

```html
function App() {
  /* existing code here, no changes */
}

/* src/App.js, change the default export to this: */
export default withAuthenticator(App);


```

Reload ứng dụng và xem kết quả, tính năng xác thực đã được thêm

![API](/images/4-auth/auth-04.png)

Chọn "Đăng ký" và làm theo hướng dẫn để tạo tài khoản. Hãy chắc chắn sử dụng một địa chỉ email thật! Sau khi gửi request đăng kí, kiểm tra email của bạn để xác nhận đăng ký.

Đã tạo thành công tính năng xác thực, bạn có thể xem nó bất kỳ lúc nào trong console bằng cách chạy lệnh sau - chọn User Pool:

```html
amplify console auth

Using service: Cognito, provided by: awscloudformation
? Which console
❯ User Pool
Identity Pool
Both
```

#### Thêm nút Đăng xuất
Update file ***App.js***

```html
function App({ signOut, user }) {
  ...
  <h1>Hello World</h1>
  ...
  <button onClick={signOut}>Sign out</button>
}
```

Thêm style tại file ***src/index.css***


```html
:root {
  --amplify-primary-color: #006eff;
  --amplify-primary-tint: #005ed9;
  --amplify-primary-shade: #005ed9;
}
```

Quan sát kết quả, nút Đăng xuất được thêm thành công

![API](/images/4-auth/auth-05.png)

#### Kiểm tra thông tin đăng nhập


Chúng ta có thể xem thông tin người dùng khi họ đã đăng nhập bằng cách gọi ***currentAuthenticatedUser()*** trong ***useEffect***:

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

Kiểm tra thông tin trên Console của Browser

![API](/images/4-auth/auth-06.png)
